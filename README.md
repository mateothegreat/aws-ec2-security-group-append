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
aws_vpc_create:
  profile: "default"            # The aws credentials file profile name.
  name: "test-1"                # Name of the vpc you want to create.
  region: "eu-central-1"        # AWS region.
  cidr: "201.0.0.0/16"          # CIDR for the entire vpc.
  internet_gateway: "true"      # Creates an internet gateway.
  nat_gateway: "true"           # Creates a nat gateway with elastic ip.
  allow_duplicate_name: "false" # Do not fail if a vpc with the same name already exists.
  allow_duplicate_cidr: "false" # Do not fail if a vpc with the same CIDR arleady exists.
  purge_cidrs: "true"           # Removes all CIDR's if using an existing vpc.
  peering:
    - name: "test-1-peer"
      vpc_id: "vpc-0504d01b93f46e610"
      region: "eu-central-1"
      cidr: "22.16.0.0/16"
      update_accepter_route_table: "true" # Add the route back to this vpc on the accepters route table.
  dns:
    enable_support: "true"      # Enable dns support for this vpc.
    enable_hostnames: "true"    # Enable hostname resolution for this vpc.
  subnets:
    - name: "public-1"
      type: "public"
      az: "eu-central-1a"
      cidr: "201.0.1.0/24"
    - name: "public-2"
      type: "public"
      cidr: "201.0.2.0/24"
      az: "eu-central-1b"
    - name: "private-1"
      type: "private"
      cidr: "201.0.3.0/24"
      az: "eu-central-1a"
    - name: "private-2"
      type: "private"
      cidr: "201.0.4.0/24"
      az: "eu-central-1b"
  tags:
    created_by: "Matthew Davis"
```

## Example Usage

Add the following to a file like `playbook.yaml`:

```yaml
- hosts: monitoring
  roles:
    - role: "mateothegreat.aws_vpc_create"
      vars:
        aws_vpc_create:
          profile: "default"            # The aws credentials file profile name.
          name: "test-1"                # Name of the vpc you want to create.
          region: "eu-central-1"        # AWS region.
          cidr: "201.0.0.0/16"          # CIDR for the entire vpc.
          internet_gateway: "true"      # Creates an internet gateway.
          nat_gateway: "true"           # Creates a nat gateway with elastic ip.
          allow_duplicate_name: "false" # Do not fail if a vpc with the same name already exists.
          allow_duplicate_cidr: "false" # Do not fail if a vpc with the same CIDR arleady exists.
          purge_cidrs: "true"           # Removes all CIDR's if using an existing vpc.
          peering:
            - name: "test-1-peer"
              vpc_id: "vpc-0504d01b93f46e610"
              region: "eu-central-1"
              cidr: "22.16.0.0/16"
              update_accepter_route_table: "true" # Add the route back to this vpc on the accepters route table.
          dns:
            enable_support: "true"      # Enable dns support for this vpc.
            enable_hostnames: "true"    # Enable hostname resolution for this vpc.
          subnets:
            - name: "public-1"
              type: "public"
              az: "eu-central-1a"
              cidr: "201.0.1.0/24"
            - name: "public-2"
              type: "public"
              cidr: "201.0.2.0/24"
              az: "eu-central-1b"
            - name: "private-1"
              type: "private"
              cidr: "201.0.3.0/24"
              az: "eu-central-1a"
            - name: "private-2"
              type: "private"
              cidr: "201.0.4.0/24"
              az: "eu-central-1b"
          tags:
            created_by: "Matthew Davis"
```

Run with `ansible-playbook -i <your inventory file> playbook.yaml`.

# Contact

You can reach out to me at https://matthewdavis.io.
