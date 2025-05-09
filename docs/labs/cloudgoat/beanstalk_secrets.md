# CloudGoat: IAM Privilege Escalation via Beanstalk Secrets

## 🧠 Scenario Summary

An attacker gains initial access to an IAM user with limited permissions and enumerates the beanstalk service secrets to elevate privileges

## ☁️ Environment Setup

Deploy CloudGoat:

```bash
cloudgoat create beanstalk_secrets
```

Authenticate with the initial credentials

```bash
aws configure --profile beanstalk
```

## 🔍 Walkthrough & Attack Path

After enumerating the Beanstalk Environment, we find access keys for a secondary user in the environment varialbes:
![Enumerate Beanstalk Secrets](../../screenshots/beanstalk_secrets/2025-04-15%2023_22_59-CloudKali%20-%20VMware%20Workstation.png)

Authenticate with the found secondary user credentials in a new profile called sec-bean
![Configure AWS](../../screenshots/beanstalk_secrets/2025-04-15%2023_24_40-CloudKali%20-%20VMware%20Workstation.png)

Check if the authentication was successful and view new identity details:
![Get Caller Identity](../../screenshots/beanstalk_secrets/2025-04-15%2023_24_55-CloudKali%20-%20VMware%20Workstation.png)

To get more details about the current principal, we use the whomai of the IAM service
![Whoami](../../screenshots/beanstalk_secrets/2025-04-15%2023_25_17-CloudKali%20-%20VMware%20Workstation.png)

Now we can check what permissions are attached to the user by viewing his policies, starting with managed policies
![List managed policies](../../screenshots/beanstalk_secrets/2025-04-15%2023_25_40-CloudKali%20-%20VMware%20Workstation.png)

We found a policy named secondary policy, let's get details about the policy and the used version:
![Policy details](../../screenshots/beanstalk_secrets/2025-04-15%2023_33_25-CloudKali%20-%20VMware%20Workstation.png)

we see that version 1 is used, let's see all versions of this policy
![policy versions](../../screenshots/beanstalk_secrets/2025-04-15%2023_33_45-CloudKali%20-%20VMware%20Workstation.png)

Now let's get the content of this specific policy version
![Policy version details](../../screenshots/beanstalk_secrets/2025-04-15%2023_35_25-CloudKali%20-%20VMware%20Workstation.png)

Boom, we have permissions to list users and create an access key for any user, let list all users in our cloud environment:
![Listing users](../../screenshots/beanstalk_secrets/2025-04-15%2023_35_51-CloudKali%20-%20VMware%20Workstation.png)

We have found an admin user, let's try to create an access key for him:
![Create admin key](../../screenshots/beanstalk_secrets/2025-04-15%2023_36_03-CloudKali%20-%20VMware%20Workstation.png)

PWND, we have now admin privileges over the cloud environment, let use the new key to authenticate with a new admin profile:
![Configure admin](../../screenshots/beanstalk_secrets/2025-04-15%2023_37_53-CloudKali%20-%20VMware%20Workstation.png)

Finally let's check the Secrets Manager for the flag:
![Beanstalk Secret](../../screenshots/beanstalk_secrets/2025-04-15%2023_38_12-CloudKali%20-%20VMware%20Workstation.png)

🛡️ That’s it for this lab — until next time, stay sharp and happy hacking!
