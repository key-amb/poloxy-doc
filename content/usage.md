+++
categories = ["general"]
date = "2016-05-14T15:01:24+09:00"
weight = 210
tags = ["document"]
title = "How to use Poloxy"

+++

## HTTP API to send alerts

Endpoint: `POST /v1/item`

Parameters:

| Key | Type | Default | Description |
|:----|:----:|:-------:|:------------|
| name | String | \- | Name of alert. Used for message title. Should be unique in _"group"_ to distinguish from others. |
| group | String | `default` | Group to categorize alerts. Nested groups are supported with delimiter `/`. |
| level | Integer | 1 | Alert level. Higher is severer. Recommended to be lower than 10. |
| type | String | \- | Method to deliver message to recipients |
| address | String | \- | Recipient address |
| message | String | \- | Message body of alert |
| misc | JSON | \- | Additional parameters for message |

### Available Delivery Types

#### `Slack`

Extends `HttpPost` to convert `message.body` to JSON format which is suitable for
[Slack Incoming WebHooks API](https://api.slack.com/incoming-webhooks).

#### `Mail`

Sends e-mail through configured SMTP server.  
This type treats `message.address` as _mail-to_.

You can add some additional parameters as `misc` contents:

| key | example | description |
|:----|:-------:|:------------|
| `mail.from` | `"foo@example.com"` | From of mail |
| `mail.cc` | `"foo@example.com,bar@example.com"` | Cc of mail |
| `mail.bcc` | `"foo@example.com,bar@example.com"` | Bcc of mail |

#### `HttpPost`

Raw HTTP POST method which posts `message.body` as request body.

### Examples of cURL Requests

Slack:

```
curl -X POST http://poloxy.yourdomain.com/v1/item -d '{
    "name": "CPU",
    "group": "generic/system-resource",
    "type": "Slack",
    "level": "3",
    "address": "https://hooks.slack.com/services/XXXXXXXXXX",
    "message": "CPU is WARNING 50%"
}'
```

Mail:

```
curl -X POST http://poloxy.yourdomain.com/v1/item -d '{
    "name": "Error Rate",
    "group": "web/rails-app",
    "type": "Mail",
    "level": "5",
    "address": "developers@yourdomain.com,admin@yourdomain.com",
    "message": "Error Rate is Over 5%",
    "misc": {
        "mail": {
            "from": "alert@yourdomain.com",
            "cc": "support@yourdomain.com",
            "bcc": "poloxy-ops@yourdomain.com"
        }
    }
}'
```

## CLI

You can use `bin/poloxy-cli` for some manual tasks.

Run `poloxy-cli help [command]` to see usage.

### Purge expired data

```bash
# purge older than PERIOD
poloxy-cli purge [--[t]ime=PERIOD]
```

Default `PERIOD` is determined by config param `data.default_keep_period`.
