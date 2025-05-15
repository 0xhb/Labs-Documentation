# CloudGoat: Vulnerable Cognito

## 🧠 Scenario Summary

Starting with access to a login and register page, exploiting coding bad practices and miscongiruations in the AWS Cognito service to gain access to AWS sensitive resources and data.

Link: [Vulnerable Cognito - Cloudgoat](https://github.com/RhinoSecurityLabs/cloudgoat/blob/master/cloudgoat/scenarios/aws/vulnerable_cognito/README.md)

## ☁️ Environment Setup

Deploy CloudGoat:

```bash
cloudgoat create vulnerable_cognito
```

This time we get a link to a web appliation login page:
![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-11%2023_38_29-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-11%2023_38_51-.png)

## 🔍 Walkthrough & Attack Path

Trying different things and exploring the application logic, let's first try to trigger the login function:

![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2000_59_02-CloudKali%20-%20VMware%20Workstation.png)

We get an error for incorrect credentials being used:

![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2000_59_33-CloudKali%20-%20VMware%20Workstation.png)

Let's navigate now to the register page and try to create an account:

![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2001_00_14-.png)

It seems that there a policy in place to only allow the domain "ecorp.com" for emails:

![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2001_00_22-CloudKali%20-%20VMware%20Workstation.png)

We still didn't check the application's source code, after viewing it, we have found some useful information about the usage of the AWS Cognito service to handle authentication for the application:

![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2000_58_21-CloudKali%20-%20VMware%20Workstation.png)

knowing that we can use the AWS CLI to interact with the service without having valid credentials, let's try to create an account from the CLI and see if we can bypass this validation:

![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2001_00_35-CloudKali%20-%20VMware%20Workstation.png)
We got a git, we can deduce that it was only client side vilidation.

We have recieved the verification code in the used temporary email:
![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2001_01_13-CloudKali%20-%20VMware%20Workstation.png)

let's confirm our account next and login to the application:
![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2001_01_25-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2001_03_04-CloudKali%20-%20VMware%20Workstation.png)

We are logged in successfully, but just as a reader, it seems that there is a kind of role based authorization, after checking the browser's storage for any cookies or tokens, we found many tokens including an access token:
![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2001_03_33-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2001_03_44-CloudKali%20-%20VMware%20Workstation.png)

Trying to get the details of the user associated with the identified token, we git a confirmation that there is a role based access control in the application:
![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2001_06_33-CloudKali%20-%20VMware%20Workstation.png)

Let's try to change the user's "custom:access" attribute to "admin":
![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2001_06_46-CloudKali%20-%20VMware%20Workstation.png)

Boom, we get another hit, so there is another role named "admin":
![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2001_07_00-CloudKali%20-%20VMware%20Workstation.png)

Let's login again and view if we can access the admin page, we only get a new message saying "You are admin", too much huh.
After tinkering for sometime with the requests, I decided to view the authentication request for the admin role in burp suite, and I was surprised that I have AWS credentials returned in the response:
![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2001_07_41-CloudKali%20-%20VMware%20Workstation.png)

Let's try another way, we know that we can use the AWS Congito Identity service to request temporary credentials to access cloud resources for authenticated users, but we need some information including the identity-pool-id, logins,and the identity id. After checking the source code for the welcome page after authentication, we find all necessary infromation there except the identity-id, but we can request this one using the AWS CLI:
![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2001_08_14-CloudKali%20-%20VMware%20Workstation.png)

We have the identity-id which is the serivce or source to provide us with the credentials, and the logins, which identifies the entity/user we want to get credentials for (using access token):
![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2001_08_31-CloudKali%20-%20VMware%20Workstation.png)
Done! We have now temporary credentials to AWS (the same ones found on burp suite).

Let's configure a new profile (cognito) with the new credentials:
![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2001_08_43-CloudKali%20-%20VMware%20Workstation.png)

We need a way to know what permissions we have using the current principal/identity, one of my favorite tools for this job is [enumerate-iam.py](https://github.com/andresriancho/enumerate-iam), let's use it right now:
![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2001_09_09-CloudKali%20-%20VMware%20Workstation.png)
We see that we have permissoins to list S3 Buckets in the cloud environment:

Trying the S3 permissions, we now have access to the application's sources code, PWND.
![Descriptive Title](../../screenshots/vulnerable_cognito/2025-05-12%2001_09_31-CloudKali%20-%20VMware%20Workstation.png)

💸 Don’t forget to destroy the environment and save those cloud credits!

```bash
cloudgoat destroy vulnerable_cognito
```

🛡️ That’s it for this lab — until next time, stay sharp and happy hacking!
