---
date: 2025-01-10
title: Why ClickHouse default logging setting is verbose?
description: "Explain why ClickHouse developers chose to set a verbose log level by default"
---

# Why ClickHouse default logging setting is verbose?

One thing that often confuses new users is that ClickHouse generates a lot of log output, even under light load. 

This is because the default log level is for historical reasons `trace` (instead of `warning` that would be the default in other databases). 

The ClickHouse developers argue that `trace` provides a lot of insight in case goes wrong. 

On the other hand, large volumes of logging means that system table `system.text_log` fills up quickly and needs to be merged in the background. 

If the database runs stable, users may re-configure the log level.

<!-- truncate -->

## Change log level

The different log level available are documented [here](https://clickhouse.com/docs/en/operations/server-configuration-parameters/settings#logger)

Edit the ClickHouse server configuration file (`/etc/clickhouse-server/config.xml`) to modify the log level. 

Default value is `trace`, change it to the desired level. 

```
<clickhouse>
    <logger>
        <!-- Possible levels [1]:

          - none (turns off logging)
          - fatal
          - critical
          - error
          - warning
          - notice
          - information
          - debug
          - trace
          - test (not for production usage)

            [1]: https://github.com/pocoproject/poco/blob/poco-1.9.4-release/Foundation/include/Poco/Logger.h#L105-L114
        -->
        <level>trace</level>
... Rest of the configuration file
</clickhouse>
```