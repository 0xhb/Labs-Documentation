# CloudGoat: IAM Privilege Escalation using AWS Lambda

## 🧠 Scenario Summary

An attacker gains initial access to an IAM user with limited permissions and enumerates IAM permissions in his environment, then escalate privileges using IAM misconfigurations through the AWS Lambda Service

Link: [Lambda PrivEsc - Cloudgoat](https://github.com/RhinoSecurityLabs/cloudgoat/blob/master/cloudgoat/scenarios/aws/lambda_privesc/README.md)

## ☁️ Environment Setup

Deploy CloudGoat:

```bash
cloudgoat create vulnerable_privesc
```

Authenticate with the initial credentials

```bash
aws configure --profile chris
```

![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_17_19-CloudKali%20-%20VMware%20Workstation.png)

## 🔍 Walkthrough & Attack Path

As always, the first thing to do is identifying what we can do and what permissions we have, so let's list the managed policies attached to our principal:

![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_54_01-CloudKali%20-%20VMware%20Workstation.png)

Look's like we have a policy named cg-chris-policy, let's list this policy versions:

![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_54_10-CloudKali%20-%20VMware%20Workstation.png)

We only have one version, v1, which is the one attached to our principal, let's get the details about this policy specific version:
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_54_21-CloudKali%20-%20VMware%20Workstation.png)
We have the permissions to list and get any IAM resource, and, more interestingly, the ability to assume a role.

Let's find out which role we can assume by listing all roles in our environment and focus on the principal field on the output:
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_54_36-CloudKali%20-%20VMware%20Workstation.png)

The first role is a lambda debug role, which may have high permissions, but can only bu assumed by the Lambda service:
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_54_56-CloudKali%20-%20VMware%20Workstation.png)

The second role, which is the one we can assume, is a lambda manager:
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_55_10-CloudKali%20-%20VMware%20Workstation.png)
Checking what permissions are attached to this role, we find that there a policy named "cg-lambdamanager" attached to the role:
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_55_34-CloudKali%20-%20VMware%20Workstation.png)

Let's find out the versions and details of the policy:
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_55_43-CloudKali%20-%20VMware%20Workstation.png)
We can see that there is only one version v1, which allows any operation on the lambda service, but also can perform PassRole, which allows us to pass a role to a lambda function in order it can execute using the permissoins of that role:
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_55_53-CloudKali%20-%20VMware%20Workstation.png)

Now I think the attack path is clear, we can first assume the lambda manager role, then create a malicious lambda function and run it in the context of a privileged role.

Let's first start by assuming the manager role and configuring an AWS profile for it:
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_56_05-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_56_18-CloudKali%20-%20VMware%20Workstation.png)

Now, let's create a malicious lambda function that will grand admin privileges to our first user, the poor chris:
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_56_50-CloudKali%20-%20VMware%20Workstation.png)

what we still need is the privileged role to pass it to the lmabda function, remember that lambda debug role, which can be used by any resource related to the lambda service, and as we know, debug == high privilege (most of the time), so let's try to use that role.

All done, now we can use the CLI to create a new lumbda function, inject our malicious payload, and pass the lambda debug role:
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_57_07-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_57_26-CloudKali%20-%20VMware%20Workstation.png)

The operation was successful, here is the output:
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_57_50-CloudKali%20-%20VMware%20Workstation.png)

Finally, let's verify the permissions our poor chris has now:
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_58_00-CloudKali%20-%20VMware%20Workstation.png)
PWND! we have AdministratorAccess to the cloud environment.

💸 Don’t forget to destroy the environment and save those cloud credits!

```bash
cloudgoat destroy lambda_privesc
```

🛡️ That’s it for this lab — until next time, stay sharp and happy hacking!
