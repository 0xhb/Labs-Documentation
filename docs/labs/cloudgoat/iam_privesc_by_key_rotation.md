# CloudGoat: IAM Privilege Escalation by Key Rotation

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

![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_14_52-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_15_20-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_15_30-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_15_39-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_15_50-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_16_03-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_16_19-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_16_37-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_17_27-.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_17_40-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_17_50-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_18_13-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_18_35-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_18_49-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_19_02-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_19_23-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_19_49-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_19_59-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_20_16-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_20_29-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_20_41-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_20_58-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_21_17-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_21_28-CloudKali%20-%20VMware%20Workstation.png)
![IAM Privilege Escalation via Key Rotation](../../screenshots/iam_privesc_by_key_rotation/2025-05-03%2015_21_37-CloudKali%20-%20VMware%20Workstation.png)

💸 Don’t forget to destroy the environment and save those cloud credits!

```bash
cloudgoat destroy vulnerable_lambda
```

🛡️ That’s it for this lab — until next time, stay sharp and happy hacking!
