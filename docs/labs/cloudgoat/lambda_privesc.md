# CloudGoat: IAM Privilege Escalation using AWS Lambda

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

![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_17_19-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_54_01-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_54_10-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_54_21-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_54_36-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_54_56-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_55_10-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_55_34-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_55_43-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_55_53-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_56_05-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_56_18-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_56_50-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_57_07-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_57_26-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_57_50-CloudKali%20-%20VMware%20Workstation.png)
![Descriptive Title](../../screenshots/lambda_privesc/2025-05-09%2015_58_00-CloudKali%20-%20VMware%20Workstation.png)

💸 Don’t forget to destroy the environment and save those cloud credits!

```bash
cloudgoat destroy vulnerable_lambda
```

🛡️ That’s it for this lab — until next time, stay sharp and happy hacking!
