# gridai-byoc-manual-eks

    - id: gridai-obj-status
      run: |
        # failed can flipped to running.  check 3 times for flip flop.
        gridai.py status_clus "${obj_id}" --min_all_match_cnt 3 --status3 "running|failed" --gha 
      shell: bash
    - run: |
        if [ "${{ steps.gridai-obj-status.outputs.match-status }}" != "running" ]; then
          exit 1
        fi             
      shell: bash


Prerequisites:
1. aws-iam-authenticator - https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html
2. terraform - https://learn.hashicorp.com/tutorials/terraform/install-cli
3. aws account
4. Python - https://www.python.org/downloads/
5. Provided htpasswd_auth and private_key

Permissions used by Grid:
"eks:*", # only for the cluster it creates
"s3:*", # only for the buckets it creates

Steps:
**Steps 4 - 10 creates the AWS infrastructure and 11 - 15 create the Grid Cluster**
x 1. Run "pip install lightning_grid --upgrade"
x 2. Get login command from "https://platform.grid.ai/#/settings?tabId=apikey"
x 3. git clone https://github.com/gridai/terraform-aws-grid-byoc-full.git
x 4. cd into terraform-aws-grid-byoc-full

5. Follow steps 1-2 of the documentation here https://docs.grid.ai/platform/upgrades/adding-custom-cloud-credentials to create a user and configure permissions. Instead of adding the IAMFUllAccess permissions, you will be using the provided policy files. The configure S3 and Configure Env policies follow the principle of least privilege and the TerraformEKSPolicy is taken from https://github.com/terraform-aws-modules/terraform-aws-eks/blob/v17.24.0/docs/iam-permissions.md with a few permissions added. If you are unsure how to add custom policies please look at the json option provided by the official aws documentation https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html.
6. Run:
unset AWS_ACCESS_KEY_ID
unset AWS_SECRET_ACCESS_KEY
unset AWS_SESSION_TOKEN
aws configure 
7. Verify AWS Access Key by running "aws sts get-caller-identity"
8. Run "terraform init"
9. Modify ./example/prod-byoc.tfvars as necessary. It is recommended to run "grid clusters" and make sure that the cluster_name in prod-byoc.tfvars does not match an existing name cluster name. Else you will receive an error.
10. Run "terraform apply --auto-approve -var-file ./example/prod-byoc.tfvars"
11. Copy output from "terraform output -json | jq -r '.gridv1_cluster.value' | tee /dev/stderr". This may output twice.
12. Run "grid clusters aws <name> --role-arn <doesn't matter> --external-id <doesn't matter> --edit-before-creation"
13. paste contents you just copied
14. Wait for the cluster to be reconciled
15. Run "grid clusters" to see clusters available
16. To set the default cluster context run "grid user set-cluster-context <cluster-name>"      