# CloudGoat: Vulnerable Lambda

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

![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2018_56_33-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2018_58_02-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_07_10-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_07_23-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_11_29-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_11_49-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_12_06-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_12_34-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_16_40-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_17_00-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_17_39-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_19_58-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_20_08-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_20_20-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_20_40-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_22_12-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_22_23-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_42_15-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_44_15-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_45_20-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_46_33-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_48_17-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_48_27-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_48_58-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_49_57-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/vulnerable_lambda/2025-05-14%2019_50_13-CloudKali%20-%20VMware%20Workstation.png)

💸 Don’t forget to destroy the environment and save those cloud credits!

```bash
cloudgoat destroy vulnerable_lambda
```

🛡️ That’s it for this lab — until next time, stay sharp and happy hacking!
