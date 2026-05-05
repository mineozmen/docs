---
description: >-
  These actions provide ability to encrypt, decrypt and hash data, as well as
  validate and generate tokens and certificates.
---

# Generate Secrets

## Generate Secrets Actions

All actions of this handler share the following event metadata parameters for getting key inputs:

{% tabs %}
{% tab title="Table" %}
| Parameter   | Definition                                | Example        | Default |
| ----------- | ----------------------------------------- | -------------- | ------- |
| Key         | Constant key to use for operations        | 1234567890ABC  | -       |
| Key Path    | Json path of key in event payload         | parameters.key | -       |
| Key Id      | ID of the key to use from key state       | 123            | -       |
| Key Id Path | Json path of key id to use from key state | parameters.id  | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "key": {
              "type": "string",
              "description": "Constant key to use for operations",
              "example": "1234567890ABC"
            },
            "keyPath": {
              "type": "string",
              "description": "Json path of key in event payload",
              "example": "parameters.key"
            },
            "keyId": {
              "type": ["string", "integer"],
              "description": "ID of the key to use from key state",
              "example": 123
            },
            "keyIdPath": {
              "type": "string",
              "description": "Json path of key id to use from key state",
              "example": "parameters.id"
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Encrypt

Encrypts a given json node or string value using preferred algorithms. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example | Default |
| -------------- | -------------------------------------------------- | ------- | ------- |
| Input Element  | Json path for the input in request event payload   | data    | -       |
| Output Element | Json path for the output in response event payload | secret  | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "description": "Json path for the input in request event payload",
          "example": "data"
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "example": "secret"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

With event metadata parameters as:

{% tabs %}
{% tab title="Table" %}
| Parameter     | Definition                        | Example | Default         |
| ------------- | --------------------------------- | ------- | --------------- |
| Algorithm     | Custom cipher algorithm to use    | -       | Handler default |
| Key Algorithm | Custom SecretKey algorithm to use | -       | Handler default |
| Provider      | Custom security provider to use   | -       | Handler default |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "algorithm": {
              "type": "string",
              "description": "Custom cipher algorithm to use",
              "default": "Handler default"
            },
            "keyAlgorithm": {
              "type": "string",
              "description": "Custom SecretKey algorithm to use",
              "default": "Handler default"
            },
            "provider": {
              "type": "string",
              "description": "Custom security provider to use",
              "default": "Handler default"
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Decrypt

Decryptes a previously encrypted value and returns as a json node or string value. This action uses the same fields as Encrypt action, with the addition of following event metadata parameter:

{% tabs %}
{% tab title="Table" %}
| Parameter | Definition                                                          | Example | Default |
| --------- | ------------------------------------------------------------------- | ------- | ------- |
| Is Json   | Whether encrypted value is json and should be parsed into an object | true    | false   |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "isJson": {
              "type": "boolean",
              "description": "Whether encrypted value is json and should be parsed into an object",
              "default": false,
              "example": true
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Hash

Hashes a given json node or string value using preferred algorithms. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example | Default |
| -------------- | -------------------------------------------------- | ------- | ------- |
| Input Element  | Json path for the input in request event payload   | data    | -       |
| Output Element | Json path for the output in response event payload | secret  | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "description": "Json path for the input in request event payload",
          "example": "data"
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "example": "secret"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

With event metadata parameters as:

{% tabs %}
{% tab title="Table" %}
| Parameter  | Definition                      | Example | Default         |
| ---------- | ------------------------------- | ------- | --------------- |
| Algorithm  | Custom hash algorithm to use    | -       | Handler default |
| Provider   | Custom security provider to use | -       | Handler default |
| Iterations | Iterations to update the hash   | 100     | 1               |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "algorithm": {
              "type": "string",
              "description": "Custom hash algorithm to use",
              "default": "Handler default"
            },
            "provider": {
              "type": "string",
              "description": "Custom security provider to use",
              "default": "Handler default"
            },
            "iterations": {
              "type": "integer",
              "description": "Iterations to update the hash",
              "default": 1,
              "example": 100
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Hash actions can be used to generate secure API keys, when used together with JmesPath salt\_key action that creates secure random key. These keys can be stored with access.roles details for key based authentication.
{% endhint %}

### ValidateHash

Validates the hash of a given json node or string value using preferred algorithms. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                                                        | Example    | Default |
| -------------- | --------------------------------------------------------------------------------- | ---------- | ------- |
| Input Element  | Json path for the input in request event payload, with "hash" and "data" elements | parameters | -       |
| Output Element | Json path for the output in response event payload                                | secret     | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "description": "Json path for the input in request event payload, with \"hash\" and \"data\" elements",
          "example": "parameters"
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "example": "secret"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

With event metadata parameters as:

{% tabs %}
{% tab title="Table" %}
| Parameter  | Definition                      | Example | Default         |
| ---------- | ------------------------------- | ------- | --------------- |
| Algorithm  | Custom hash algorithm to use    | -       | Handler default |
| Provider   | Custom security provider to use | -       | Handler default |
| Iterations | Iterations to update the hash   | 100     | 1               |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "algorithm": {
              "type": "string",
              "description": "Custom hash algorithm to use",
              "default": "Handler default"
            },
            "provider": {
              "type": "string",
              "description": "Custom security provider to use",
              "default": "Handler default"
            },
            "iterations": {
              "type": "integer",
              "description": "Iterations to update the hash",
              "default": 1,
              "example": 100
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### GenerateToken

Generates a JWT token for given claims (including special claims such as audience). Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                             | Example    | Default |
| -------------- | ------------------------------------------------------ | ---------- | ------- |
| Input Element  | Json path for the fields to include as claims in token | parameters | -       |
| Output Element | Json path to add token at                              | secret     | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "description": "Json path for the fields to include as claims in token",
          "example": "parameters"
        },
        "outputElement": {
          "type": "string",
          "description": "Json path to add token at",
          "example": "secret"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

With event metadata parameters as:

{% tabs %}
{% tab title="Table" %}
| Parameter       | Definition                          | Example | Default         |
| --------------- | ----------------------------------- | ------- | --------------- |
| Provider        | Custom security provider to use     | -       | Handler default |
| Expiration Time | Milliseconds to expiration of token | 60000   | 0               |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "provider": {
              "type": "string",
              "description": "Custom security provider to use",
              "default": "Handler default"
            },
            "expirationTime": {
              "type": "integer",
              "description": "Milliseconds to expiration of token",
              "default": 0,
              "example": 60000
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### ValidateToken

Validates a JWT token. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                            | Example          | Default |
| -------------- | ------------------------------------- | ---------------- | ------- |
| Input Element  | Json path for the token               | parameters.token | -       |
| Output Element | Json path to add validation result to | isValid          | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "description": "Json path for the token",
          "example": "parameters.token"
        },
        "outputElement": {
          "type": "string",
          "description": "Json path to add validation result to",
          "example": "isValid"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

With event metadata parameters as:

{% tabs %}
{% tab title="Table" %}
| Parameter     | Definition                                    | Example | Default         |
| ------------- | --------------------------------------------- | ------- | --------------- |
| Provider      | Custom security provider to use               | -       | Handler default |
| Input Pattern | Jmespath expression to apply on input element | -       | -               |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "provider": {
              "type": "string",
              "description": "Custom security provider to use",
              "default": "Handler default"
            },
            "inputPattern": {
              "type": "string",
              "description": "Jmespath expression to apply on input element"
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### DecodeToken

Decodes a JWT token and returns its claims. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                         | Example          | Default |
| -------------- | ---------------------------------- | ---------------- | ------- |
| Input Element  | Json path for the token            | parameters.token | -       |
| Output Element | Json path to add decoded claims to | claims           | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "description": "Json path for the token",
          "example": "parameters.token"
        },
        "outputElement": {
          "type": "string",
          "description": "Json path to add decoded claims to",
          "example": "claims"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

With event metadata parameters as:

{% tabs %}
{% tab title="Table" %}
| Parameter     | Definition                                    | Example | Default         |
| ------------- | --------------------------------------------- | ------- | --------------- |
| Provider      | Custom security provider to use               | -       | Handler default |
| Input Pattern | Jmespath expression to apply on input element | -       | -               |
| Validate      | Whether the token must be valid to decode     | false   | true            |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "provider": {
              "type": "string",
              "description": "Custom security provider to use",
              "default": "Handler default"
            },
            "inputPattern": {
              "type": "string",
              "description": "Jmespath expression to apply on input element"
            },
            "validate": {
              "type": "boolean",
              "description": "Whether the token must be valid to decode",
              "default": true,
              "example": false
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### GenerateCertificate

Generates a certificate, returning private key and public certificate values. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                          | Example         | Default |
| -------------- | --------------------------------------------------- | --------------- | ------- |
| Input Element  | Json path for custom certificate DN and lifetime    | parameters.cert | -       |
| Output Element | Json path to add "key" and "certificate" outputs to | produced        | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "description": "Json path for custom certificate DN and lifetime",
          "example": "parameters.cert"
        },
        "outputElement": {
          "type": "string",
          "description": "Json path to add \"key\" and \"certificate\" outputs to",
          "example": "produced"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

With event metadata parameters as:

{% tabs %}
{% tab title="Table" %}
| Parameter                      | Definition                          | Example | Default         |
| ------------------------------ | ----------------------------------- | ------- | --------------- |
| Provider                       | Custom security provider to use     | -       | Handler default |
| Certificate Algorithm          | Custom certificate algorithm        | -       | Handler default |
| Certificate SignatureAlgorithm | Custom signature algorithm          | -       | Handler default |
| Certificate Key Size           | Custom key size                     | -       | Handler default |
| Certificate DN                 | Custom certificate DN               | -       | Handler default |
| Certificate Life Time          | Custom certificate lifetime in days | -       | Handler default |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "provider": {
              "type": "string",
              "description": "Custom security provider to use",
              "default": "Handler default"
            },
            "certificateAlgorithm": {
              "type": "string",
              "description": "Custom certificate algorithm",
              "default": "Handler default"
            },
            "certificateSignatureAlgorithm": {
              "type": "string",
              "description": "Custom signature algorithm",
              "default": "Handler default"
            },
            "certificateKeySize": {
              "type": ["integer", "string"],
              "description": "Custom key size",
              "default": "Handler default"
            },
            "certificateDN": {
              "type": "string",
              "description": "Custom certificate DN",
              "default": "Handler default"
            },
            "certificateLifeTime": {
              "type": ["integer", "string"],
              "description": "Custom certificate lifetime in days",
              "default": "Handler default"
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}
