In this lab, we will exploit misconfigured Google Cloud Storage (GCS) bucket policies to gain administrative access to one of the storage buckets.

## Lab Walkthrough

### Phase 1: Reconnaissance and Discovery

#### 1. Identify Storage Buckets from the Web Application

Navigate to the GCPGoat application and open any blog post from the homepage.

![GCPGoat application homepage](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab02-01.png)

#### 2. Locate Cloud-Hosted Resources

Right-click on an image in the blog post and open it in a new tab.

![Blog post with image](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab02-02.png)

#### 3. Extract Bucket Information

Examine the image URL in the address bar, noting that it's served from a Google Cloud Storage bucket.

![Image URL showing Google Cloud Storage bucket](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab02-03.png)

Extract the bucket name that appears after `storage.googleapis.com/` in the URL.

### Phase 2: Initial Access Testing

#### 4. Test Production Bucket Access

Attempt to list the bucket's contents by visiting:

```
https://storage.googleapis.com/<PROD_BUCKET_NAME>
```

![Access denied error for production bucket](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab02-04.png)

You'll receive an "Access Denied" error since public access to the bucket listings is typically restricted.

#### 5. Evaluate Bucket Permissions

Use the TestIamPermissions API to determine what permissions you currently have on the production bucket:

```
https://www.googleapis.com/storage/v1/b/<PROD_BUCKET_NAME>/iam/testPermissions?permissions=storage.buckets.delete&permissions=storage.buckets.get&permissions=storage.buckets.getIamPolicy&permissions=storage.buckets.setIamPolicy&permissions=storage.buckets.update&permissions=storage.objects.create&permissions=storage.objects.delete&permissions=storage.objects.get&permissions=storage.objects.list&permissions=storage.objects.update
```

![Test IAM permissions for production bucket](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab02-05.png)

The results show limited permissions and only `storage.buckets.get`.

### Phase 3: Discovering Development Environment

#### 6. Apply the Dev Environment Naming Pattern

Organizations often use consistent naming patterns across environments. We will try accessing a development version of the bucket by replacing "prod" with "dev" in the bucket name:

```
https://storage.googleapis.com/<DEV_BUCKET_NAME>
```

![Access denied for development bucket](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab02-06.png)

We receive an "Access Denied" error, but this confirms the existence of a development bucket.

#### 7. Check Development Bucket Permissions

Test permissions on the development bucket using the same TestIamPermissions API:

```
https://www.googleapis.com/storage/v1/b/<DEV_BUCKET_NAME>/iam/testPermissions?permissions=storage.buckets.delete&permissions=storage.buckets.get&permissions=storage.buckets.getIamPolicy&permissions=storage.buckets.setIamPolicy&permissions=storage.buckets.update&permissions=storage.objects.create&permissions=storage.objects.delete&permissions=storage.objects.get&permissions=storage.objects.list&permissions=storage.objects.update
```

![Test IAM permissions for development bucket showing expanded access](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab02-07.png)

We notice that the development bucket has more permissive settings, granting:
- `storage.buckets.getIamPolicy`: Ability to view IAM policies
- `storage.buckets.setIamPolicy`: Ability to modify IAM policies

These additional permissions represent a significant security misconfiguration.

### Phase 4: Privilege Escalation

#### 8. Examine Current IAM Policy

We will use the `gsutil` command-line tool to view the current IAM policy of the development bucket:

```bash
gsutil iam get gs://<DEV_BUCKET_NAME>
```

![Current IAM policy for development bucket](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab02-08.png)

The output reveals that `allUsers` (public anonymous access) already has some permissions through a custom role.

#### 9. Escalate Privileges

After this, we will try to grant admin access to all anonymous users this command:

```bash
gsutil iam ch allUsers:admin gs://<DEV_BUCKET_NAME>
```

![Adding admin role to allUsers](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab02-09.png)

#### 10. Verify Privilege Escalation

We'll use the next command to confirm that the changes were applied successfully:

```bash
gsutil iam get gs://<DEV_BUCKET_NAME>
```

![Updated IAM policy showing admin access](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab02-10.png)

We notice that `allUsers` have been granted the `admin` role.

### Phase 5: Exploiting Admin Access

#### 11. Confirm Extended Permissions

Now, we use the TestIamPermissions API again to check the new permissions:

```
https://www.googleapis.com/storage/v1/b/<DEV_BUCKET_NAME>/iam/testPermissions?permissions=storage.buckets.delete&permissions=storage.buckets.get&permissions=storage.buckets.getIamPolicy&permissions=storage.buckets.setIamPolicy&permissions=storage.buckets.update&permissions=storage.objects.create&permissions=storage.objects.delete&permissions=storage.objects.get&permissions=storage.objects.list&permissions=storage.objects.update
```

![Expanded permissions after privilege escalation](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab02-11.png)

We see that we got full access to all requested permissions, including object listing, creation, update, and deletion.

#### 12. Access Bucket Contents

Now, we can browse the contents of the development bucket, and view all the objects stored in the bucket:

```
https://storage.googleapis.com/<DEV_BUCKET_NAME>
```

![Successfully listing bucket contents](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab02-12.png)
