# Google Cloud IAM Commands

## Get all roles associated with a service account

```bash
gcloud projects get-iam-policy <project name> --flatten='bindings[].members' --format='table(bindings.role)' --filter='bindings.members:<service account email>' --format=json
```
