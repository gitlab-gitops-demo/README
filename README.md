# GitOps

This example group is intended to show how to use `git` and GitLab as a single 
source of truth for Infrastructure and Application management.

### Group Structure
```
├─ gitops-demo
    ├── readme
    ├── gitlab-manage
    ├── apps
    │   ├── my-asp-net-app1
    │   ├── my-dotnet-app1
    │   ├── my-python-app4
    │   ├── my-ruby-app3
    │   └── my-spring-app2
    └── infra
        ├── aws
        ├── gcp
        └── templates
```

### Visual Representation
![GitOps-Demo.svg](GitOps-Demo.svg)

### Repository Purpose

*readme* - documentation of the gitops-demo group, and this file.

*gitlab-manage* - terraform code to represent the gitops-demo group configuration on gitlab.com.

*apps/\** - application and CI/CD code, one per application.

*infra/\** - terraform code to represent each cloud's configuration (vpc, security groups, and cluster configuration)



### To reproduce this demo within your own group
1. Create a top level group in GitLab (i.e. gitops-demo)
1. Create an organization (i.e. gitops-demo) and workspace named `gitlab-manage` on `https://app.terraform.io`. Change the Workspace Execution mode to `Local`
1. Clone the [GitLab Manage](https://gitlab.com/gitops-demo/gitlab-manage) repository into that group.
    * Add an Environment Variable to this project called `GITLAB_TOKEN` with a personal access token with rights to the top level group.
    * Add an Environment File to this project called `TF_CLI_CONFIG_FILE` with the following format for a value.
        ```
        credentials "app.terraform.io" {token = "my-token-from-tf-cloud"}
        ```

    * Adjust the groups and user names in the *.tf files.
    * Run the CI to deploy all the changes.
1. Clone all the projects into the proper groups.
1. Add the following Environment Variables to the `infra` group.
| Variable Name | 
| ------ | 
| AWS_ACCESS_KEY_ID | 
| AWS_REGION | 
| AWS_SECRET_ACCESS_KEY |
| GITLAB_TOKEN |
| GOOGLE_CREDENTIALS |
| GOOGLE_PROJECT |
| GOOGLE_REGION |
| TF_CLI_CONFIG_FILE |

1. Run the CI to create the infrastructure.
1. Run CI on each project to deploy each application.

#### Clean up
1. To clean up the infrastructure, invoke the manual action for `destroy` on each of the aws and gcp projects.

