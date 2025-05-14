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

![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-11%2023_38_29-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-11%2023_38_51-.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2000_58_21-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2000_59_02-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2000_59_33-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2001_00_14-.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2001_00_22-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2001_00_35-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2001_01_13-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2001_01_25-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2001_03_04-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2001_03_33-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2001_03_44-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2001_06_33-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2001_06_46-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2001_07_00-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2001_07_41-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2001_08_14-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2001_08_31-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2001_08_43-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2001_09_09-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/beanstalk_secrets/2025-05-12%2001_09_31-CloudKali%20-%20VMware%20Workstation.png)

💸 Don’t forget to destroy the environment and save those cloud credits!

```bash
cloudgoat destroy beanstalk_secrets
```

🛡️ That’s it for this lab — until next time, stay sharp and happy hacking!
