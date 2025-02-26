---
page_title: "Resource: auth0_trigger_binding"
description: |-
  With this resource, you can bind actions to a trigger. Once actions are created and deployed, they can be attached (i.e. bound) to a trigger so that it will be executed as part of a flow. The list of actions reflects the order in which they will be executed during the appropriate flow.
---

# Resource: auth0_trigger_binding

With this resource, you can bind actions to a trigger. Once actions are created and deployed, they can be attached (i.e. bound) to a trigger so that it will be executed as part of a flow. The list of actions reflects the order in which they will be executed during the appropriate flow.

!> This resource has been deprecated in favor of the `auth0_trigger_actions` resource.

## Example Usage

```terraform
resource "auth0_action" "action_foo" {
  name   = "Test Trigger Binding Foo"
  code   = <<-EOT
    exports.onContinuePostLogin = async (event, api) => {
      console.log("foo");
    };"
	EOT
  deploy = true

  supported_triggers {
    id      = "post-login"
    version = "v3"
  }
}

resource "auth0_action" "action_bar" {
  name   = "Test Trigger Binding Bar"
  code   = <<-EOT
    exports.onContinuePostLogin = async (event, api) => {
      console.log("bar");
    };"
	EOT
  deploy = true

  supported_triggers {
    id      = "post-login"
    version = "v3"
  }
}

resource "auth0_trigger_binding" "login_flow" {
  trigger = "post-login"

  actions {
    id           = auth0_action.action_foo.id
    display_name = auth0_action.action_foo.name
  }

  actions {
    id           = auth0_action.action_bar.id
    display_name = auth0_action.action_bar.name
  }
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `actions` (Block List, Min: 1) The list of actions bound to this trigger. (see [below for nested schema](#nestedblock--actions))
- `trigger` (String) The ID of the trigger to bind with.

### Read-Only

- `id` (String) The ID of this resource.

<a id="nestedblock--actions"></a>
### Nested Schema for `actions`

Required:

- `display_name` (String) The display name of the action within the flow.
- `id` (String) Action ID.

## Import

Import is supported using the following syntax:

```shell
# This resource can be imported using the bindings trigger ID.
#
# Example:
terraform import auth0_trigger_binding.example "post-login"
```
