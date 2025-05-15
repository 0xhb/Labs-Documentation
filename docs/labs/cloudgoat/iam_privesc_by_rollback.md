# CloudGoat: IAM Privilege Escalation by Rollback

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

![IAM Privilege Escalation via Rollback](../../screenshots/iam_privesc_by_rollback/2025-05-04%2012_00_33-Kyuu-Ji_Awesome-Azure-Pentest_%20A%20collection%20of%20resources,%20tools%20and%20more%20for%20pen.png)
![IAM Privilege Escalation via Rollback](../../screenshots/iam_privesc_by_rollback/2025-05-09%2015_02_02-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Rollback](../../screenshots/iam_privesc_by_rollback/2025-05-09%2015_08_57-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Rollback](../../screenshots/iam_privesc_by_rollback/2025-05-09%2015_09_09-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Rollback](../../screenshots/iam_privesc_by_rollback/2025-05-09%2015_09_27-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Rollback](../../screenshots/iam_privesc_by_rollback/2025-05-09%2015_09_50-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Rollback](../../screenshots/iam_privesc_by_rollback/2025-05-09%2015_10_06-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Rollback](../../screenshots/iam_privesc_by_rollback/2025-05-09%2015_10_20-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Rollback](../../screenshots/iam_privesc_by_rollback/2025-05-09%2015_10_33-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Rollback](../../screenshots/iam_privesc_by_rollback/2025-05-09%2015_10_44-CloudKali%20-%20VMware%20Workstation.png)

💸 Don’t forget to destroy the environment and save those cloud credits!

```bash
cloudgoat destroy vulnerable_lambda
```

🛡️ That’s it for this lab — until next time, stay sharp and happy hacking!
