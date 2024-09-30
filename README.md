# Checkov Custom Rules

## Skipping Rules On Repository Level

Inside `infra/terraform` create config file named `.checkov.yml` and define checks to skip:
```
skip-check: 
  - CKV_DOCKER_3 
  - CKV_DOCKER_2 
```
