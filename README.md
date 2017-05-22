tf_aws_aurora
===========

Terraform module for deploying and managing AWS Aurora

This module

- we encrypt everything and make a kms id for you
- future enhancement to make kms_key_id optional, so if you do pass it it'll use it, and if you don't it'll create the kms key id for you

----------------------
#### Required


- `azs` - "A list of Availability Zones in the Region"
- `cluster_size` - "Number of cluster instances to create"
- `env` - "env to deploy into, should typically dev/staging/prod"
- `name` - "Name for the Redis replication group i.e. cmsCommon"
- `subnets` - "List of subnet IDs to use in creating RDS subnet group (must already exist)"
- `vpc_id`  - "VPC ID"

##### see [aws_rds_cluster documentation](https://www.terraform.io/docs/providers/aws/r/rds_cluster.html) for these variables
- `database_name`
- `master_username`
- `master_password`

#### Optional
- `backup_retention_period` - "The days to retain backups for defaults to 30"
- `db_port` - "defaults to 3306"
- `allowed_cidr` - "A list of Security Group ID's to allow access to. Defaults to ["127.0.0.1/32"]"
- `allowed_security_groups` - "A list of Security Group ID's to allow access to. Defaults to empty list"
- `preferred_backup_window` - "The daily time range in UTC during which automated backups are created. Default to 01:00-03:00"
- `instance_class` - "Instance class to use when creating RDS cluster. Defaults to db.t2.medium"
- `storage_encrypted` - "Defaults to true"
- `apply_immediately` - "Defaults to false"

Usage
-----

```hcl

module "aurora" {
  source = "github.com/terraform-community-modules/tf_aws_aurora?ref=1.0.0"
  azs                     = "${var.azs}"
  env                     = "${var.env}"
  name                    = "thtest"
  subnets                 = "${module.vpc.database_subnets}"
  vpc_id                  = "${module.vpc.vpc_id}"
  cluster_size            = "2"
  allowed_cidr            = ["10.10.10.0/24", "10.10.20.0/24"]
  allowed_security_groups = ["sg-24f4655b"]
  database_name           = "thtest"
  master_username         = "tim"
  master_password         = "test1234"
  backup_retention_period = "1"
  #preferred_backup_window = ""
}

```

Outputs
=======
- `rds_cluster_id`
- `writer_endpoint`
- `reader_endpoint`
- `security_group_id`


Authors
=======

[Tim Hartmann](https://github.com/tfhartmann)

License
=======
