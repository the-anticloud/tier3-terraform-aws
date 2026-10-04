# Deploy Guide — terraform-aws
**Platform:** Anticloud sovereign infrastructure | Air-gap capable
**Stack:** Terraform 1.7+, AWS Provider, VPC (no IGW), AIOSS_FORMAT

## Prerequisites
Python 3.11+. See stack: Terraform 1.7+, AWS Provider, VPC (no IGW), AIOSS_FORMAT. AIOSS_FORMAT required. PAX 27B weights for AI-assisted features.

## AIOSS Integration
```bash
aioss init --module terraform-aws --output ./terraform_aws.aioss
aioss append --chain ./terraform_aws.aioss --payload ./output.bin --module terraform-aws
aioss verify --chain ./terraform_aws.aioss
```

## Air-Gap Deployment
```bash
# On networked machine:
pip download -r requirements.txt -d ./wheels/
# Transfer wheels/ to air-gap host, then:
pip install --no-index --find-links ./wheels/ -r requirements.txt
```

## PAX 27B Harness Wiring
```python
from anticloud_pax import PAXHarness
harness = PAXHarness(
    model_path="./pax-27b-q4.gguf",
    module="terraform-aws",
    aioss_chain="./terraform_aws.aioss",
    classification="L5_NARROW_L2_GENERAL"
)
result = harness.process(input_data)
```

## Verification
```bash
aioss verify --chain ./terraform_aws.aioss --verbose
python -m terraform_aws.tests.smoke
```
