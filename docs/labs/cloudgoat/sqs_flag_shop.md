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

![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_00_11-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_00_33-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_04_01-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_04_23-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_04_38-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_05_54-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_07_13-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_09_29-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_14_16-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_14_33-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_14_48-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_14_57-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_16_58-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_17_07-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_29_10-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_29_31-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_31_09-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_31_34-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_31_44-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_32_06-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_32_16-CloudKali%20-%20VMware%20Workstation.png)
![SQS Flag Shop](../../screenshots/sqs_flag_shop/2025-05-11%2012_32_46-CloudKali%20-%20VMware%20Workstation.png)

💸 Don’t forget to destroy the environment and save those cloud credits!

```bash
cloudgoat destroy vulnerable_lambda
```

🛡️ That’s it for this lab — until next time, stay sharp and happy hacking!
