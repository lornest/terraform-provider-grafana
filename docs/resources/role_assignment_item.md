---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "grafana_role_assignment_item Resource - terraform-provider-grafana"
subcategory: "Grafana Enterprise"
description: |-
  Manages a single assignment for a role. Conflicts with the "grafana_role_assignment" resource which manages the entire set of assignments for a role.
---

# grafana_role_assignment_item (Resource)

Manages a single assignment for a role. Conflicts with the "grafana_role_assignment" resource which manages the entire set of assignments for a role.

## Example Usage

```terraform
resource "grafana_role" "test_role" {
  name    = "Test Role"
  uid     = "testrole"
  version = 1
  global  = true

  permissions {
    action = "org.users:add"
    scope  = "users:*"
  }
}

resource "grafana_team" "test_team" {
  name = "terraform_test_team"
}

resource "grafana_user" "test_user" {
  email    = "terraform_user@test.com"
  login    = "terraform_user@test.com"
  password = "password"
}

resource "grafana_service_account" "test_sa" {
  name = "terraform_test_sa"
  role = "Viewer"
}

resource "grafana_role_assignment_item" "user" {
  role_uid = grafana_role.test_role.uid
  user_id  = grafana_user.test_user.id
}

resource "grafana_role_assignment_item" "team" {
  role_uid = grafana_role.test_role.uid
  team_id  = grafana_team.test_team.id
}

resource "grafana_role_assignment_item" "service_account" {
  role_uid           = grafana_role.test_role.uid
  service_account_id = grafana_service_account.test_sa.id
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `role_uid` (String) the role UID onto which to assign an actor

### Optional

- `org_id` (String) The Organization ID. If not set, the Org ID defined in the provider block will be used.
- `service_account_id` (String) the service account onto which the role is to be assigned
- `team_id` (String) the team onto which the role is to be assigned
- `user_id` (String) the user onto which the role is to be assigned

### Read-Only

- `id` (String) The ID of this resource.

## Import

Import is supported using the following syntax:

```shell
terraform import grafana_role_assignment_item.name "{{ roleUID }}:{{ type (user, team or service_account) }}:{{ identifier }}"
terraform import grafana_role_assignment_item.name "{{ orgID }}:{{ roleUID }}:{{ type (user, team or service_account) }}:{{ identifier }}"
```