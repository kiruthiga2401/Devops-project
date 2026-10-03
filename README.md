## 📌What this project does

When you stop using an EC2 server, its snapshots (backups) stay in AWS and you keep paying for them. This project uses a small Python program on AWS Lambda to find those unused snapshots and delete them, so your bill goes down.
## 🔍Identifying Stale EBS Snapshots
In this example, we'll create a Lambda function that identifies EBS snapshots that are no longer associated with any active EC2 instance and deletes them to save on storage costs.
## 🛠️ Execution Steps
### 🖥️1. Create an EC2 instance.
   This is a server on AWS. It comes with a disk called an EBS volume.
  <img width="1897" height="493" alt="Screenshot 2026-10-03 115853" src="https://github.com/user-attachments/assets/ef10c0db-0128-47ab-9841-83046ff9a02d" />
### 📸2. Create a snapshot.
A snapshot is a backup copy of that disk. AWS charges money to store it.
<img width="1606" height="371" alt="Screenshot 2026-10-03 120029" src="https://github.com/user-attachments/assets/54761263-f22d-4ec0-b557-bf5c8f257c41" />

### ⚡3. Create the Lambda function.
Lambda runs your code serverlessly. Paste the Python code and click **Deploy**.
> 💡 **Troubleshooting:** 
<img width="1491" height="537" alt="Screenshot 2026-10-03 120758" src="https://github.com/user-attachments/assets/6060760c-0a5e-4491-ab5c-4cf1221cb13d" />

-> The first run failed because Lambda stops the code after 3 seconds by default.
-> You changed the timeout to 10 seconds, and then it worked.
### 4. 🔑 Assign IAM Permissions
Lambda cannot use EC2 unless you allow it. You make a policy and attach it to the Lambda role. 

It needs four permissions:
- `DescribeSnapshots` lists the snapshots.
- `DescribeInstances` finds the active instances.
- `DescribeVolumes` checks whether a snapshot's volume still exists and is attached.
- `DeleteSnapshot` removes the stale ones.

### 5. 🚀 Run the Function
The code checks every snapshot. If the snapshot's disk was deleted, or no running server uses it, the snapshot is deleted. After the run you check the EC2 dashboard and the old snapshot is gone.
<img width="1052" height="456" alt="Screenshot 2026-10-03 121349" src="https://github.com/user-attachments/assets/92a6116a-4e27-46a0-9f85-7b37ae3cd3a2" />

## 🧪 Real-World Case Study
1. Launched one EC2 instance and created two snapshots.
2. Terminated the server, leaving one snapshot orphaned.
3. Triggered the Lambda function:
   - Initial execution failed due to the default **3-second timeout**.
   - Increased timeout to **10 seconds** and re-ran.
4. **Result:** The stale snapshot was deleted automatically, while the active instance's snapshot remained intact.
<img width="1032" height="312" alt="Screenshot 2026-10-03 121602" src="https://github.com/user-attachments/assets/868984bc-f7bb-460f-9f9e-11c7e18c3664" />

