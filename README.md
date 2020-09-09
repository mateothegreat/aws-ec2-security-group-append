# Create AWS VPC

Features:

* Creates (optional) peering connection(s).
* Supports tagging for ALL resources.


This role will create a new AWS VPC including:

* Peering Connection(s)
* Internet Gateway
* NAT Gateway(s)
* Route Table(s)
* Subnet(s)

## Requirements

```bash
pip install ansible boto3 boto
```
## Variables

```yaml
aws_ec2_security_group_append:
   profile: "mlfabric"
   name: "mlfabric-monitoring-agents-ingress"
   description: "monitoring-agents-ingress"
   vpc_id: "vpc-123"
   ports:
     - "8081"
   rule_desc: "monitoring agent"
```

## Example Usage

Add the following to a file like `playbook.yaml`:

```yaml
- hosts: monitoring
  roles:
    - role: "mateothegreat.aws_vpc_create"
      vars:
        aws_ec2_security_group_append:
            profile: "mlfabric"
            name: "mlfabric-monitoring-agents-ingress"
            description: "monitoring-agents-ingress"
            vpc_id: "vpc-123"
            ports:
              - "8081"
            rule_desc: "monitoring agent"
```

Run with `ansible-playbook -i <your inventory file> playbook.yaml`.

# Contact

You can reach out to me at https://matthewdavis.io.
