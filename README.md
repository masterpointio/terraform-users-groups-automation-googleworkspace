[![Banner][banner-image]](https://masterpoint.io/)

# terraform-googleworkspace-users-groups-automation

[![Release][release-badge]][latest-release]

💡 Learn more about Masterpoint [below](#who-we-are-𐦂𖨆𐀪𖠋).

## Purpose and Functionality

Use this [child module](https://opentofu.org/docs/language/modules/#child-modules) to manage Google Workspace users, groups, and roles.

If you want to use this module with an existing Google Workspace, see the [import-existing-org](examples/import-existing-org) example, which demonstrates how to import your existing Google users and groups.

## Usage

### Step-by-Step Instructions

There are two provider authentication methods available:

1. Authenticate using a Google Cloud service account key file.
2. Authenticate using a Google Cloud service account key file and impersonate a real user with Super Admin privileges.

We recommend method (2), impersonating a Super Admin, as this allows you to grant Admin privileges to users (service accounts cannot do this). To set this up, follow the [Domain-Wide Delegation authentication instructions](https://github.com/hashicorp/terraform-provider-googleworkspace/blob/main/docs/index.md#using-domain-wide-delegation).

Once you've completed the setup process, your provider block should look like this:

```hcl
provider "googleworkspace" {
  # Use 'my_customer' as an alias for your account's customerId to ensure compatibility with Google's API
  # For example, custom schemas on the user object will fail if the customer_id is set to your actual customer_id
  # For more details: https://developers.google.com/workspace/admin/directory/reference/rest/v1/schemas/get
  customer_id = "my_customer"

  credentials             = "/path/to/credentials/my-google-project-credentials-1234567890.json"
  impersonated_user_email = "my_impersonated_user_email@my_domain.com"

  oauth_scopes = [
    "https://www.googleapis.com/auth/admin.directory.group",
    "https://www.googleapis.com/auth/admin.directory.user",
    "https://www.googleapis.com/auth/admin.directory.userschema",
    "https://www.googleapis.com/auth/apps.groups.settings",
    "https://www.googleapis.com/auth/iam",
  ]
}
```

## Example

```hcl
module "googleworkspace_users_groups" {
  source = "git::https://github.com/masterpointio/terraform-users-groups-automation-googleworkspace.git"

  users = {
    "first.last@example.com" = {
      primary_email = "first.last@example.com"
      family_name   = "Last"
      given_name    = "First"
      password      = "example-password"
      groups = {
        "platform" = {
          role = "member"
        }
      }
    }
  }

  groups = {
    "platform" = {
      name     = "Platform"
      email    = "platform@example.com"
      settings = {
        who_can_join = "ALL_IN_DOMAIN_CAN_JOIN"
      }
    }
  }
}
```

<!-- prettier-ignore-start -->
<!-- markdownlint-disable MD013 -->
<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.0 |
| <a name="requirement_googleworkspace"></a> [googleworkspace](#requirement\_googleworkspace) | >= 0.7.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_googleworkspace"></a> [googleworkspace](#provider\_googleworkspace) | >= 0.7.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_this"></a> [this](#module\_this) | cloudposse/label/null | 0.25.0 |

## Resources

| Name | Type |
|------|------|
| [googleworkspace_group.defaults](https://registry.terraform.io/providers/hashicorp/googleworkspace/latest/docs/resources/group) | resource |
| [googleworkspace_group_member.user_to_groups](https://registry.terraform.io/providers/hashicorp/googleworkspace/latest/docs/resources/group_member) | resource |
| [googleworkspace_group_settings.defaults](https://registry.terraform.io/providers/hashicorp/googleworkspace/latest/docs/resources/group_settings) | resource |
| [googleworkspace_user.defaults](https://registry.terraform.io/providers/hashicorp/googleworkspace/latest/docs/resources/user) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_additional_tag_map"></a> [additional\_tag\_map](#input\_additional\_tag\_map) | Additional key-value pairs to add to each map in `tags_as_list_of_maps`. Not added to `tags` or `id`.<br/>This is for some rare cases where resources want additional configuration of tags<br/>and therefore take a list of maps with tag key, value, and additional configuration. | `map(string)` | `{}` | no |
| <a name="input_attributes"></a> [attributes](#input\_attributes) | ID element. Additional attributes (e.g. `workers` or `cluster`) to add to `id`,<br/>in the order they appear in the list. New attributes are appended to the<br/>end of the list. The elements of the list are joined by the `delimiter`<br/>and treated as a single ID element. | `list(string)` | `[]` | no |
| <a name="input_context"></a> [context](#input\_context) | Single object for setting entire context at once.<br/>See description of individual variables for details.<br/>Leave string and numeric variables as `null` to use default value.<br/>Individual variable settings (non-null) override settings in context object,<br/>except for attributes, tags, and additional\_tag\_map, which are merged. | `any` | <pre>{<br/>  "additional_tag_map": {},<br/>  "attributes": [],<br/>  "delimiter": null,<br/>  "descriptor_formats": {},<br/>  "enabled": true,<br/>  "environment": null,<br/>  "id_length_limit": null,<br/>  "label_key_case": null,<br/>  "label_order": [],<br/>  "label_value_case": null,<br/>  "labels_as_tags": [<br/>    "unset"<br/>  ],<br/>  "name": null,<br/>  "namespace": null,<br/>  "regex_replace_chars": null,<br/>  "stage": null,<br/>  "tags": {},<br/>  "tenant": null<br/>}</pre> | no |
| <a name="input_delimiter"></a> [delimiter](#input\_delimiter) | Delimiter to be used between ID elements.<br/>Defaults to `-` (hyphen). Set to `""` to use no delimiter at all. | `string` | `null` | no |
| <a name="input_descriptor_formats"></a> [descriptor\_formats](#input\_descriptor\_formats) | Describe additional descriptors to be output in the `descriptors` output map.<br/>Map of maps. Keys are names of descriptors. Values are maps of the form<br/>`{<br/>   format = string<br/>   labels = list(string)<br/>}`<br/>(Type is `any` so the map values can later be enhanced to provide additional options.)<br/>`format` is a Terraform format string to be passed to the `format()` function.<br/>`labels` is a list of labels, in order, to pass to `format()` function.<br/>Label values will be normalized before being passed to `format()` so they will be<br/>identical to how they appear in `id`.<br/>Default is `{}` (`descriptors` output will be empty). | `any` | `{}` | no |
| <a name="input_enabled"></a> [enabled](#input\_enabled) | Set to false to prevent the module from creating any resources | `bool` | `null` | no |
| <a name="input_environment"></a> [environment](#input\_environment) | ID element. Usually used for region e.g. 'uw2', 'us-west-2', OR role 'prod', 'staging', 'dev', 'UAT' | `string` | `null` | no |
| <a name="input_groups"></a> [groups](#input\_groups) | List of groups | <pre>map(object({<br/>    name : string,<br/>    description : optional(string),<br/>    email : string,<br/>    timeouts : optional(object({<br/>      create : optional(string),<br/>      update : optional(string),<br/>      }), {<br/>      create = null<br/>      update = null<br/>    }),<br/>    # https://registry.terraform.io/providers/hashicorp/googleworkspace/latest/docs/resources/group_settings<br/>    settings : optional(object({<br/>      allow_external_members : optional(bool),<br/>      allow_web_posting : optional(bool),<br/>      archive_only : optional(bool),<br/>      custom_footer_text : optional(string),<br/>      custom_reply_to : optional(string),<br/>      default_message_deny_notification_text : optional(string),<br/>      enable_collaborative_inbox : optional(bool),<br/>      include_custom_footer : optional(bool),<br/>      include_in_global_address_list : optional(bool),<br/>      is_archived : optional(bool),<br/>      members_can_post_as_the_group : optional(bool),<br/>      message_moderation_level : optional(string),<br/>      primary_language : optional(string),<br/>      reply_to : optional(string),<br/>      send_message_deny_notification : optional(bool),<br/>      spam_moderation_level : optional(string),<br/>      who_can_assist_content : optional(string),<br/>      who_can_contact_owner : optional(string),<br/>      who_can_discover_group : optional(string),<br/>      who_can_join : optional(string),<br/>      who_can_leave_group : optional(string),<br/>      who_can_moderate_content : optional(string),<br/>      who_can_moderate_members : optional(string),<br/>      who_can_post_message : optional(string),<br/>      who_can_view_group : optional(string),<br/>      who_can_view_membership : optional(string),<br/>    }), {}),<br/>  }))</pre> | `{}` | no |
| <a name="input_id_length_limit"></a> [id\_length\_limit](#input\_id\_length\_limit) | Limit `id` to this many characters (minimum 6).<br/>Set to `0` for unlimited length.<br/>Set to `null` for keep the existing setting, which defaults to `0`.<br/>Does not affect `id_full`. | `number` | `null` | no |
| <a name="input_label_key_case"></a> [label\_key\_case](#input\_label\_key\_case) | Controls the letter case of the `tags` keys (label names) for tags generated by this module.<br/>Does not affect keys of tags passed in via the `tags` input.<br/>Possible values: `lower`, `title`, `upper`.<br/>Default value: `title`. | `string` | `null` | no |
| <a name="input_label_order"></a> [label\_order](#input\_label\_order) | The order in which the labels (ID elements) appear in the `id`.<br/>Defaults to ["namespace", "environment", "stage", "name", "attributes"].<br/>You can omit any of the 6 labels ("tenant" is the 6th), but at least one must be present. | `list(string)` | `null` | no |
| <a name="input_label_value_case"></a> [label\_value\_case](#input\_label\_value\_case) | Controls the letter case of ID elements (labels) as included in `id`,<br/>set as tag values, and output by this module individually.<br/>Does not affect values of tags passed in via the `tags` input.<br/>Possible values: `lower`, `title`, `upper` and `none` (no transformation).<br/>Set this to `title` and set `delimiter` to `""` to yield Pascal Case IDs.<br/>Default value: `lower`. | `string` | `null` | no |
| <a name="input_labels_as_tags"></a> [labels\_as\_tags](#input\_labels\_as\_tags) | Set of labels (ID elements) to include as tags in the `tags` output.<br/>Default is to include all labels.<br/>Tags with empty values will not be included in the `tags` output.<br/>Set to `[]` to suppress all generated tags.<br/>**Notes:**<br/>  The value of the `name` tag, if included, will be the `id`, not the `name`.<br/>  Unlike other `null-label` inputs, the initial setting of `labels_as_tags` cannot be<br/>  changed in later chained modules. Attempts to change it will be silently ignored. | `set(string)` | <pre>[<br/>  "default"<br/>]</pre> | no |
| <a name="input_name"></a> [name](#input\_name) | ID element. Usually the component or solution name, e.g. 'app' or 'jenkins'.<br/>This is the only ID element not also included as a `tag`.<br/>The "name" tag is set to the full `id` string. There is no tag with the value of the `name` input. | `string` | `null` | no |
| <a name="input_namespace"></a> [namespace](#input\_namespace) | ID element. Usually an abbreviation of your organization name, e.g. 'eg' or 'cp', to help ensure generated IDs are globally unique | `string` | `null` | no |
| <a name="input_regex_replace_chars"></a> [regex\_replace\_chars](#input\_regex\_replace\_chars) | Terraform regular expression (regex) string.<br/>Characters matching the regex will be removed from the ID elements.<br/>If not set, `"/[^a-zA-Z0-9-]/"` is used to remove all characters other than hyphens, letters and digits. | `string` | `null` | no |
| <a name="input_stage"></a> [stage](#input\_stage) | ID element. Usually used to indicate role, e.g. 'prod', 'staging', 'source', 'build', 'test', 'deploy', 'release' | `string` | `null` | no |
| <a name="input_tags"></a> [tags](#input\_tags) | Additional tags (e.g. `{'BusinessUnit': 'XYZ'}`).<br/>Neither the tag keys nor the tag values will be modified by this module. | `map(string)` | `{}` | no |
| <a name="input_tenant"></a> [tenant](#input\_tenant) | ID element \_(Rarely used, not included by default)\_. A customer identifier, indicating who this instance of a resource is for | `string` | `null` | no |
| <a name="input_users"></a> [users](#input\_users) | List of users | <pre>map(object({<br/>    # addresses<br/>    aliases : optional(list(string), []),<br/>    archived : optional(bool, false),<br/>    change_password_at_next_login : optional(bool),<br/>    # custom_schemas<br/>    # emails<br/>    # external_ids<br/>    family_name : string,<br/>    given_name : string,<br/>    groups : optional(map(object({<br/>      role : optional(string, "MEMBER"),<br/>      delivery_settings : optional(string, "ALL_MAIL"),<br/>      type : optional(string, "USER"),<br/>    })), {}),<br/>    # ims<br/>    include_in_global_address_list : optional(bool),<br/>    ip_allowlist : optional(bool),<br/>    is_admin : optional(bool),<br/>    # keywords<br/>    # languages<br/>    # locations<br/>    org_unit_path : optional(string),<br/>    # organizations<br/>    # phones<br/>    # posix_accounts<br/>    primary_email : string,<br/>    recovery_email : optional(string),<br/>    recovery_phone : optional(string),<br/>    # relations<br/>    # ssh_public_keys<br/>    suspended : optional(bool),<br/>    # timeouts<br/>    # websites<br/><br/>    # User attributes with unique constraints<br/><br/>    # password and hash_function<br/>    # If a hashFunction is specified, the password must be a valid hash key.<br/>    # If it's not specified, the password should be in clear text and between<br/>    # 8–100 ASCII characters.<br/>    # https://developers.google.com/workspace/admin/directory/v1/guides/manage-users<br/>    hash_function : optional(string),<br/>    password : optional(string),<br/>  }))</pre> | `{}` | no |

## Outputs

No outputs.
<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
<!-- markdownlint-enable MD013 -->
<!-- prettier-ignore-end -->

## Built By

Powered by the [Masterpoint team](https://masterpoint.io/who-we-are/) and driven forward by contributions from the community ❤️

[![Contributors][contributors-image]][contributors-url]

## Contribution Guidelines

Contributions are welcome and appreciated!

Found an issue or want to request a feature? [Open an issue][issues-url]

Want to fix a bug you found or add some functionality? Fork, clone, commit, push, and PR — we'll check it out.

## Who We Are 𐦂𖨆𐀪𖠋

Established in 2016, Masterpoint is a team of experienced software and platform engineers specializing in Infrastructure as Code (IaC). We provide expert guidance to organizations of all sizes, helping them leverage the latest IaC practices to accelerate their engineering teams.

### Our Mission

Our mission is to simplify cloud infrastructure so developers can innovate faster, safer, and with greater confidence. By open-sourcing tools and modules that we use internally, we aim to contribute back to the community, promoting consistency, quality, and security.

### Our Commitments

- 🌟 **Open Source**: We live and breathe open source, contributing to and maintaining hundreds of projects across multiple organizations.
- 🌎 **1% for the Planet**: Demonstrating our commitment to environmental sustainability, we are proud members of [1% for the Planet](https://www.onepercentfortheplanet.org), pledging to donate 1% of our annual sales to environmental nonprofits.
- 🇺🇦 **1% Towards Ukraine**: With team members and friends affected by the ongoing [Russo-Ukrainian war](https://en.wikipedia.org/wiki/Russo-Ukrainian_War), we donate 1% of our annual revenue to invasion relief efforts, supporting organizations providing aid to those in need. [Here's how you can help Ukraine with just a few clicks](https://masterpoint.io/updates/supporting-ukraine/).

## Connect With Us

We're active members of the community and are always publishing content, giving talks, and sharing our hard earned expertise. Here are a few ways you can see what we're up to:

[![LinkedIn][linkedin-badge]][linkedin-url] [![Newsletter][newsletter-badge]][newsletter-url] [![Blog][blog-badge]][blog-url] [![YouTube][youtube-badge]][youtube-url]

... and be sure to connect with our founder, [Matt Gowie](https://www.linkedin.com/in/gowiem/).

## License

[Apache License, Version 2.0][license-url].

[![Open Source Initiative][osi-image]][license-url]

Copyright © 2016-2025 [Masterpoint Consulting LLC](https://masterpoint.io/)

<!-- MARKDOWN LINKS & IMAGES -->

[banner-image]: https://masterpoint-public.s3.us-west-2.amazonaws.com/v2/standard-long-fullcolor.png
[license-url]: https://opensource.org/license/apache-2-0
[osi-image]: https://i0.wp.com/opensource.org/wp-content/uploads/2023/03/cropped-OSI-horizontal-large.png?fit=250%2C229&ssl=1
[linkedin-badge]: https://img.shields.io/badge/LinkedIn-Follow-0A66C2?style=for-the-badge&logoColor=white
[linkedin-url]: https://www.linkedin.com/company/masterpoint-consulting
[blog-badge]: https://img.shields.io/badge/Blog-IaC_Insights-55C1B4?style=for-the-badge&logoColor=white
[blog-url]: https://masterpoint.io/updates/
[newsletter-badge]: https://img.shields.io/badge/Newsletter-Subscribe-ECE295?style=for-the-badge&logoColor=222222
[newsletter-url]: https://newsletter.masterpoint.io/
[youtube-badge]: https://img.shields.io/badge/YouTube-Subscribe-D191BF?style=for-the-badge&logo=youtube&logoColor=white
[youtube-url]: https://www.youtube.com/channel/UCeeDaO2NREVlPy9Plqx-9JQ
[release-badge]: https://img.shields.io/github/v/release/masterpointio/terraform-users-groups-automation-googleworkspace?color=0E383A&label=Release&style=for-the-badge&logo=github&logoColor=white
[latest-release]: https://github.com/masterpointio/terraform-users-groups-automation-googleworkspace/releases/latest
[contributors-image]: https://contrib.rocks/image?repo=masterpointio/terraform-users-groups-automation-googleworkspace
[contributors-url]: https://github.com/masterpointio/terraform-users-groups-automation-googleworkspace/graphs/contributors
[issues-url]: https://github.com/masterpointio/terraform-users-groups-automation-googleworkspace/issues
