Grid.ai BYOC Customer Manged tfvar short explanation

These are subject to frequent changes.

### Basic Configurations

- the following must be set

```json
cluster_name    = "c211115-221335"
hostname        = "c211115-221335.prod.grid.cloud"
region          = "us-east-1"
```

- instances to configure 

```json
instances = [
  {
    name : "t2.medium",
    overprovisioned_ondemand_count : 2,
  },
  {
    name : "m5a.large",
    overprovisioned_ondemand_count : 2,
  },
  {
    name : "g4dn.xlarge",
    overprovisioned_ondemand_count : 0,
  },
]
```


- TODO: need explanations

```json
bucket_name             = ""
dns_firewall            = false
testing_cluster         = false
```

### Performance and Scale

- control scalability 
```json
# true = 1 node for mgt
# false = 5 nodes mgt
cost_saving_mode = true 
```

- minimum recommended instance types 

```json
management_node_instance_type  = "r5a.2xlarge"
tensorboard_node_instance_type = "t3a.medium"
builder_node_instance_type     = "m5a.xlarge"
```

### Security

- security audit

```json
guard_duty_integration  = true
```

- security network 

```json
tailscale_token         = ""
tailscale_subnet        = ""
```

- security AMI overrides 

```json
cpu_ami_override      = ""
gpu_ami_override      = ""
builder_ami_override  = ""
```

### AWS Specific

- Grid.ai AWS Principal

```json
grid_account_id  = "302180240179"
```

- EKS Version
https://docs.aws.amazon.com/eks/latest/userguide/kubernetes-versions.html

```json
cluster_version = "1.20"
```

### Eventual Deprecation

- variables that will eventually deprecate

```json
multi_az_count  = 2
```

- the following are not used and dummy values should be placed

```json
role_arn    = "ignored"
external_id = "ignored"
```

