# Developer Cookbook — terraform-aws
**Stack:** Terraform 1.7+, AWS Provider, VPC (no IGW), AIOSS_FORMAT
**Domain:** Sovereign AWS deployment: air-gapped VPC with no external internet (outpost/GovCloud)
**License:** Apache-2.0 | **IP:** USPTO pending 2026, Anticloud FZ LLE

## Core Usage

```bash
# Plan deployment
terraform plan -out=anticloud.tfplan

# PAX validate plan
anticloud tool pax --prompt 'Validate this Terraform plan for sovereign compliance (no IGW, encrypted storage)' \
  --file anticloud.tfplan.json --max-tokens 1024

# Apply
terraform apply anticloud.tfplan

# Verify: no external routes
aws ec2 describe-route-tables --filters 'Name=route.gateway-id,Values=igw-*'
# Should return empty — no IGW routes
```

## AIOSS Chain Append

```python
import hashlib, time

def aioss_append(chain_path, payload: bytes, module_id: str):
    entry_hash = hashlib.sha3_256(payload).digest()
    ts = int(time.time_ns()).to_bytes(8, 'big')
    with open(chain_path, 'rb') as f:
        f.seek(-32, 2); prev_hash = f.read(32)
    new_hash = hashlib.sha3_256(prev_hash + entry_hash + ts).digest()
    with open(chain_path, 'ab') as f:
        f.write(ts + entry_hash + new_hash)
    return new_hash.hex()

# After every terraform-aws output:
chain_hash = aioss_append("./terraform_aws.aioss",
                           result_bytes, "terraform-aws")
```

## Performance & Integration

Performance: profile with api-oss-devtools. Benchmark with api-oss-analytics. Integration: all terraform-aws operations are logged to api-oss-logging and audited by api-oss-compliance.
