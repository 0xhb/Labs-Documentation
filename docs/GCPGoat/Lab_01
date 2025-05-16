In this lab, we will exploit a Server-Side Request Forgery (SSRF) vulnerability in a cloud function to extract sensitive information including:
1. System files from the function environment
2. Source code of the cloud function itself
3. Database credentials and user data

To avoid any problems, try opening in a private window.

Using this information, we'll escalate privileges to gain administrative access to a web application.

## Lab Walkthrough

### Phase 1: Initial Access and Vulnerability Discovery

#### 1. Access the Application
Register a new user account in the web application, then log in with your credentials.

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-01.png)

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-02.png)

#### 2. Identify the SSRF Vector
Navigate to the "Newpost" section from the side navigation menu.

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-03.png)

Create a new post with the following details:
- Headline: "SSRF test" (or any test title)
- Author's Name: [Your name]
- Image URL: `file:///etc/passwd` (This is our SSRF payload)

#### 3. Capture and Analyze the Response
Before submitting the form:
1. Open the browser's developer tools (Right-click > Inspect element)
2. Select the "Network" tab to monitor HTTP requests
3. Click the "Upload" button to submit the form

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-04.png)

You should see a "URL File uploaded successfully" notification at the bottom of the screen.

#### 4. Verify SSRF Vulnerability
In the Network tab of the developer tools:
1. Find the upload request
2. Open its response details
3. Copy the returned URL
4. Open this URL in a new browser tab

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-05.png)

The browser will display or download the contents of the `/etc/passwd` file, confirming that the SSRF vulnerability exists.

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-06.png)

### Phase 2: Information Gathering

#### 5. Extract Cloud Function Environment Details
Return to the "Newpost" form and repeat the process with a new payload:

```
file:///proc/self/environ
```

This file contains environment variables set for the cloud function.

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-07.png)

After successful upload, open or download the file using the response URL.

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-08.png)

If the file downloads instead of displaying in the browser, use the terminal to view its contents:

```bash
cat [downloaded-file-name]
```

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-09.png)

From this output, you can identify:
- GCP project name
- JWT_SECRET value
- Runtime environment (Python)
- Other configuration details

#### 6. Extract Cloud Function Source Code
Return to the "Newpost" form and use the following payload:

```
file:///workspace/main.py
```

This targets the source code file of the Python cloud function, which is typically located in the `/workspace` directory.

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-10.png)

After successful upload, retrieve the file using the response URL.

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-11.png)

The downloaded file contains the entire source code of the cloud function. Upon inspection, you'll discover a hidden development endpoint.

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-12.png)

This endpoint (`/dump-db-321423541325`) appears to dump the entire database, which is a significant security issue.

### Phase 3: Privilege Escalation

#### 7. Extract the Function URL and Access the Debug Endpoint
From the developer tools:
1. Identify the cloud function's base URL from the request headers

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-13.png)

2. Construct the debug endpoint URL by appending the path discovered in the source code:

```
<FunctionURL>/dump-db-321423541325
```

3. Access this URL in your browser

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-14.png)

The response contains the entire database contents in JSON format, including user credentials.

#### 8. Identify Admin Account
Analyze the database dump to locate an account with administrative privileges (`auth_level: 0`):

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-15.png)

Note the following details for this admin account:
- Email address
- Security question
- Security answer (stored in plaintext)

#### 9. Perform Password Reset
Navigate to the login page and initiate a password reset:

1. Click "Forgot Password"

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-16.png)

2. Enter the admin email address and answer the security question using the information from the database dump
3. Set a new password of your choice

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-17.png)

You should receive a "Password changed successfully" confirmation.

#### 10. Access Admin Account
Return to the login page and sign in with:
- Email: [Admin email from database]
- Password: [Your new password]

![](GCPGoat%20Screenshots/GCPGoat-Lab02/GCPGoat-Lab01-18.png)

You now have administrative access to the application.

![Admin dashboard](https://user-images.githubusercontent.com/54552051/204803483-a48bceae-69f3-45d7-b24e-f83a039ab3b9.png)

Navigate to the "User" section to confirm your administrative privileges.

![](https://user-images.githubusercontent.com/54552051/204803485-60af1708-3aa1-4380-b2a0-f23dfd5ea243.png)
