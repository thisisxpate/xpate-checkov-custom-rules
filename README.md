# Checkov Custom Rules

## Skipping Rules On Repository Level

Inside `infra/terraform` create config file named `.checkov.yml` and define checks to skip:
```
skip-check: 
  - CKV_DOCKER_3 
  - CKV_DOCKER_2 
```

## Implementing New Checks

After you implement new checks, you can test them on your repository by adding 
```
      checkov_custom_rules_ref: 0.0.1
```
to the Terraform workflow on your repository, e.g.
```
  terraform-plan-apply:
    uses: thisisxpate/xpate-infrastructure-shared/.github/workflows/terraform-plan-apply.yaml@add-ref-to-checkov
    needs: setup
    with:
      deployment_ci_role: ${{ needs.setup.outputs.deployment_ci_role }}
      account_name: ${{ needs.setup.outputs.account_name }}
      env: ${{ needs.setup.outputs.env }}
      lock_id: ${{ needs.setup.outputs.lock_id }}
      auto_apply: ${{ needs.setup.outputs.auto_apply }}
      slack_channel: ${{ needs.setup.outputs.slack_channel }}
      checkov_custom_rules_ref: 0.0.1
    secrets: inherit
```
Reference can be tag or branch, after releasing new check (merge into main), create new tag and specify it in `xpate-infrastructure-shared` repository in shared workflow e.g.
```
.github/workflows/terraform-plan-apply.yaml (xpate-infrastructure-shared repository)
...
name: Terraform Plan & Apply

on:
  workflow_call:
    inputs:
      runs_on:
        required: false
        type: string
        default: "ubuntu-latest"
...
      checkov_custom_rules_ref:
        description: "Which Version Of Custom Checkov Rules Should Be Used"
        required: false
        type: string
        default: "0.0.1" <= use new tag by default in all workflows
```
