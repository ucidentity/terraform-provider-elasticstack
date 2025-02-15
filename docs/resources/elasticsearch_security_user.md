---
subcategory: "Security"
layout: ""
page_title: "Elasticstack: elasticstack_elasticsearch_security_user Resource"
description: |-
  Adds and updates users in the native realm.
---

# Resource: elasticstack_elasticsearch_security_user

Adds and updates users in the native realm. These users are commonly referred to as native users. See, https://www.elastic.co/guide/en/elasticsearch/reference/current/security-api-put-user.html

## Example Usage

```terraform
provider "elasticstack" {
  elasticsearch {}
}

resource "elasticstack_elasticsearch_security_user" "user" {
  username = "testuser"

  // use hashed password: see https://www.elastic.co/guide/en/elasticsearch/reference/current/security-api-put-user.html#security-api-put-user-request-body
  password_hash = "$2a$10$rMZe6TdsUwBX/TA8vRDz0OLwKAZeCzXM4jT3tfCjpSTB8HoFuq8xO"
  roles         = ["kibana_user"]

  // set the custom metadata for this user
  metadata = jsonencode({
    "env"    = "testing"
    "open"   = false
    "number" = 49
  })

  elasticsearch_connection {
    endpoints = ["http://localhost:9200"]
    username  = "elastic"
    password  = "changeme"
  }
}

resource "elasticstack_elasticsearch_security_user" "dev" {
  username = "devuser"

  password = "1234567890"
  roles    = ["kibana_user"]

  // set the custom metadata for this user
  metadata = jsonencode({
    "env"    = "testing"
    "open"   = false
    "number" = 49
  })
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- **username** (String) An identifier for the user (see https://www.elastic.co/guide/en/elasticsearch/reference/current/security-api-put-user.html#security-api-put-user-path-params).

### Optional

- **elasticsearch_connection** (Block List, Max: 1) Used to establish connection to Elasticsearch server. Overrides environment variables if present. (see [below for nested schema](#nestedblock--elasticsearch_connection))
- **email** (String) The email of the user.
- **enabled** (Boolean) Specifies whether the user is enabled. The default value is true.
- **full_name** (String) The full name of the user.
- **id** (String) The ID of this resource.
- **metadata** (String) Arbitrary metadata that you want to associate with the user.
- **password** (String, Sensitive) The user’s password. Passwords must be at least 6 characters long.
- **password_hash** (String, Sensitive) A hash of the user’s password. This must be produced using the same hashing algorithm as has been configured for password storage (see https://www.elastic.co/guide/en/elasticsearch/reference/current/security-settings.html#hashing-settings).
- **roles** (Set of String) A set of roles the user has. The roles determine the user’s access permissions. Default is [].

<a id="nestedblock--elasticsearch_connection"></a>
### Nested Schema for `elasticsearch_connection`

Optional:

- **endpoints** (List of String, Sensitive) A list of endpoints the Terraform provider will point to. They must include the http(s) schema and port number.
- **password** (String, Sensitive) A password to use for API authentication to Elasticsearch.
- **username** (String) A username to use for API authentication to Elasticsearch.

## Import

Import is supported using the following syntax:

```shell
terraform import elasticstack_elasticsearch_security_user.user <cluster_uuid>/elastic
```
