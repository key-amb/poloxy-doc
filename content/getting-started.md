+++
categories = ["general"]
date = "2016-05-14T14:51:40+09:00"
weight = 200
tags = ["document"]
title = "Getting Started"

+++

## Prerequisites

- Ruby ... _v2.0.0_ or later
- An RDBMS supported by [Sequel](https://github.com/jeremyevans/sequel) ORM
  - Tested by SQLite3 and developed with MySQL. Hopefully other RDBs will do.

## Install

```
gem install bundler
git clone https://github.com/progrhyme/poloxy.git
cd poloxy
bundle install
```

Or, you can get released revisions from [Releases](https://github.com/progrhyme/poloxy/releases)
pages.

## Configure

### Configuration File

Default configuration file is `config/poloxy.toml`.  
And this file is included in this repository as a sample.

You can change its path by `POLOXY_CONFIG` environment variable.

Here are configuration items:

| Item | Type | Default | Description |
|:-----|:----:|:-------:|:------------|
| log.level | String | `INFO` | Log level for std-lib `Logger` class |
| log.rotate | String | \- | `shift_age` param for `Logger#new` |
| log.file | String | \- | Path to log output file |
| deliver.min_interval | Period | `1 min` | Minimum interval period to deliver summarized alerts to recipients |
| deliver.item.merger | String | `PerItem` | Method to summarize alerts. See following section. |
| database.connect | Hash | \- | Params to connect database. These params are passed to `Sequel#connect`. |
| smtp.host | String | `localhost` | SMTP server address used for `Mail` delivery-type |
| smtp.port | Integer | 25 | SMTP server port used for `Mail` delivery-type |
| message.default_expire | Period | `2 hour` | Period to expire alerts. Expired alerts are taken as "CLEAR". |
| message.default_snooze | Period | `30 min` | Period to snooze alerts. See following section. |
| data.default_keep_period | Period | `2 day` | Default period to hold data |

As for type _Period_, natural time expressions are supported powered by
[chronic_duration](https://github.com/hpoydar/chronic_duration).  
Numeric expressions like `60` are taken as seconds.

#### Alerts Snoozing

When an alert occurs, subsequent alerts with the same (_group_, _name_, _level_)
are snoozed for configured `message.default_snooze` seconds.

#### Available Parameters

Some config items have alternatives which are specific to **poloxy**.  
They are described in the table below:

| Item | Option | Description |
|:-----|:------:|:------------|
| deliver.item.merger | `PerItem` | Summarize alert items by unique (_address_, _type_, _group_, _name_) params of them |
| | `PerGroup` | Summarize by unique (_address_, _type_, _group_) of alerts |
| | `PerAddress` | Summarize by unique (_address_, _type_) of alerts |

As for parameters of alert items, see [API description](#http-api-to-send-alerts) in latter section.

### Database Schema

Database schema is bundled under `db/migrate/` directory in the repository.  
And migration task is defined in `Rakefile`.

Run:

```
rake db:migrate
```

Then you will get prepared with `poloxy` database.

NOTE:

- You should prepare `database.connect` parameter in configuration file beforehand.
- Run `rake db:reset` to destroy `poloxy` database.
- Your database adapter such as [mysql2](https://rubygems.org/gems/mysql2) is not included in Gemfile.
So `bundle exec db:*` will fail unless you use SQLite3.


## Run "poloxy" Server

```
# Start Web/API
bin/poloxy-webapi

# Start Worker
bin/poloxy-worker
```

Web/API server runs at port 4567 by default.

Access server on your browser and you will see **poloxy dashboard**.

Then, let's go on to [How to use Poloxy](usage) to send alerts to **poloxy**.
