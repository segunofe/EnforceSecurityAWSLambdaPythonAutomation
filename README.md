# Automate Security Compliance in AWS using Python & Lambda

## Overview

This project automatically detects and terminates EC2 instances with SSH open to the public, using a Python Lambda function triggered by EventBridge.

---
<img width="1376" height="768" alt="python AWS Lambda EventBridge" src="https://github.com/user-attachments/assets/18ba6be0-a897-4ab7-add5-3df91c41d53a" />
  ```
   https://youtu.be/V0LfQPkyyM0
   ```


## Step 1: Write the Python Lambda Function

Write your Python compliance logic and wrap it in a Lambda handler function:

```python
def lambda_handler(event, context):
    # your code here
```

> **Tip:** Select all your existing code and press `Tab` to indent it inside the handler body.

---

## Step 2: Package and Upload to S3

1. Zip your Python file:
   ```bash
   zip open-ssh-ec2-check.zip open-ssh-ec2-check.py
   ```
2. Upload the `.zip` to an S3 bucket.
3. Copy the **Object URL** — you'll need it in Step 4.

---

## Step 3: Create an IAM Role for Lambda

1. Go to **IAM > Roles > Create role**
2. Trusted entity: **AWS Service**
3. Use case: **Lambda**
4. Role name: `dev-role-lambda`
5. Attach the following policies:
   - `AmazonEC2FullAccess`
   - `AWSLambdaBasicExecutionRole`

---

## Step 4: Create the Lambda Function

1. **Name:** `open-ssh-ec2-check`
2. **Runtime:** Python 3.12
3. **Permissions:** Use the role created in Step 3 (`dev-role-lambda`)
4. Delete the default placeholder code
5. Under **Code source**, choose **Upload from Amazon S3 location** and paste the Object URL → **Save**
6. Update the handler path: **Configuration > Edit > Handler** → set to:
   ```
   open-ssh-ec2-check.lambda_handler
   ```
7. Increase the timeout: **Configuration > General configuration > Edit > Timeout** → `30 seconds` → **Save**
8. Run a **Test** to confirm the function executes correctly.

---

## Step 5: Add an EventBridge Trigger

1. In the Lambda function, go to **Add trigger**
2. Select **EventBridge (CloudWatch Events)**
3. Choose **Create a new rule** with the following settings:

| Field | Value |
|---|---|
| Rule name | `open-ssh-ec2-check` |
| Description | Checks for EC2 instances with SSH open to the public and terminates them |
| Rule type | Event pattern |
| Service | EC2 |
| Event type | EC2 Instance State-change Notification |
| Detail – State | `running` |

4. Click **Add**

---

## Troubleshooting: EventBridge Trigger Not Firing

### Problem
The Lambda function worked correctly in isolation, but the EventBridge trigger was not invoking it.

### Debugging Steps

1. **Verified Lambda worked independently** — manual test passed without issues.
2. **Tested with AWS API call via CloudTrail** — created an EC2 instance to observe if CloudTrail events triggered the function.
3. **Discovered a missing CloudTrail trail** — researched the issue and found that this event pattern requires an active trail to be configured.
4. **Evaluated the CloudTrail approach** — creating a trail requires a dedicated S3 bucket for log storage, and costs would grow as log volume increased.
5. **Switched to EC2 state-change notifications** — changed the EventBridge rule to use **EC2 Instance State-change Notification** with the `detail.state = "running"` filter instead of relying on CloudTrail.
6. **Confirmed the fix** — after updating the rule, the EventBridge trigger fired as expected when a new EC2 instance entered the `running` state.

### Root Cause
The original rule used **AWS API call via CloudTrail**, which requires an active CloudTrail trail to generate events. Switching to the native **EC2 Instance State-change Notification** event type eliminated the dependency on CloudTrail entirely.

---
## Watch it here on YouTube

  ```bash
   https://youtu.be/V0LfQPkyyM0
   ```
https://youtu.be/V0LfQPkyyM0
### Python Code Credit to Azeez Salu
