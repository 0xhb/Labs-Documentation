# CloudGoat: IAM Privilege Escalation via Beanstalk Secrets

## 🧠 Scenario Summary

An attacker gains initial access to an IAM user with limited permissions and enumerates the beanstalk service secrets to elevate privileges

## ☁️ Environment Setup

Deploy CloudGoat:

```bash
cloudgoat create vulnerable_lambda
```

Authenticate with the initial credentials

```bash
aws configure --profile beanstalk
```

## 🔍 Walkthrough & Attack Path

![SNS Secrets](../../screenshots/sns_secrets/2025-04-17%2010_59_13-PwnCloudOS%20-%20VMware%20Workstation.png)
![SNS Secrets](../../screenshots/sns_secrets/2025-04-17%2010_59_31-PwnCloudOS%20-%20VMware%20Workstation.png)
![SNS Secrets](../../screenshots/sns_secrets/2025-04-17%2010_59_43-PwnCloudOS%20-%20VMware%20Workstation.png)
![SNS Secrets](../../screenshots/sns_secrets/2025-04-17%2011_00_22-PwnCloudOS%20-%20VMware%20Workstation.png)
![SNS Secrets](../../screenshots/sns_secrets/2025-04-17%2011_00_37-PwnCloudOS%20-%20VMware%20Workstation.png)
![SNS Secrets](../../screenshots/sns_secrets/2025-04-17%2011_00_57-PwnCloudOS%20-%20VMware%20Workstation.png)
![SNS Secrets](../../screenshots/sns_secrets/2025-04-17%2011_01_11-PwnCloudOS%20-%20VMware%20Workstation.png)
![SNS Secrets](../../screenshots/sns_secrets/2025-04-17%2011_01_28-PwnCloudOS%20-%20VMware%20Workstation.png)
![SNS Secrets](../../screenshots/sns_secrets/2025-04-17%2011_01_50-PwnCloudOS%20-%20VMware%20Workstation.png)
![SNS Secrets](../../screenshots/sns_secrets/2025-04-17%2011_02_16-PwnCloudOS%20-%20VMware%20Workstation.png)
![SNS Secrets](../../screenshots/sns_secrets/2025-04-17%2011_02_28-PwnCloudOS%20-%20VMware%20Workstation.png)
![SNS Secrets](../../screenshots/sns_secrets/2025-04-17%2011_02_40-PwnCloudOS%20-%20VMware%20Workstation.png)
![SNS Secrets](../../screenshots/sns_secrets/2025-04-17%2011_02_55-PwnCloudOS%20-%20VMware%20Workstation.png)
![SNS Secrets](../../screenshots/sns_secrets/2025-04-17%2011_03_07-PwnCloudOS%20-%20VMware%20Workstation.png)
![SNS Secrets](../../screenshots/sns_secrets/2025-04-17%2011_03_31-PwnCloudOS%20-%20VMware%20Workstation.png)

💸 Don’t forget to destroy the environment and save those cloud credits!

```bash
cloudgoat destroy vulnerable_lambda
```

🛡️ That’s it for this lab — until next time, stay sharp and happy hacking!
