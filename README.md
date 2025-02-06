<h1>Aws-Cost-Optimization</h1>

AWS Cloud Cost Optimization – Identifying Stale EBS Snapshots
Why Identify Stale EBS Snapshots?
Amazon Elastic Block Store (EBS) snapshots are incremental backups of EBS volumes stored in Amazon S3. Over time, businesses accumulate numerous snapshots, leading to high storage costs if old, unused snapshots remain.

Since snapshots are priced per GB-month, it's essential to periodically audit and delete those no longer needed to optimize costs.

<h2> How to Identify Stale EBS Snapshots?</h2>
A stale snapshot is a backup that is no longer linked to an active EC2 instance or volume. Typically, snapshots are created manually or through AWS Backup, but if an EC2 instance or EBS volume is deleted, its associated snapshots may remain indefinitely, incurring unnecessary costs.

<h4>To identify stale snapshots, we follow these steps:</h4>
1.Retrieve all EBS snapshots owned by the account.
2.Fetch all active EC2 instances (both running and stopped) and extract the associated volumes.
3Compare the snapshot’s source volume with active EC2 volumes:
<li>If the snapshot references a volume that no longer exists or is not attached to an instance, it's considered stale.</li>
<li>Delete the stale snapshots to free up storage.</li>


<h2> Automating Cleanup with AWS Lambda </h2>
AWS Lambda, combined with AWS SDK (Boto3 for Python), allows us to automate this process efficiently. The Lambda function performs the following actions:

Step 1: Fetch All EBS Snapshots
Use the 'describe_snapshots' API to get a list of all snapshots created by the account ('OwnerIds=['self']').
Step 2: Fetch All Active EC2 Instances & Volumes
Use 'describe_instances' to get all EC2 instances that are either running or stopped.
Extract volume IDs attached to these instances using 'describe_volumes'.
Step 3: Identify Stale Snapshots
For each snapshot, check if its source volume still exists.
If the volume is missing from the active instance list, the snapshot is stale.
Step 4: Delete Stale Snapshots
Use 'delete_snapshot' to remove snapshots that meet the stale criteria.
