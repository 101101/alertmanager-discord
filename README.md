# alertmanager-discord  

> A minimal docker image with golang application, which listens for Prometheus Alertmanager's notifications and pushes them to a Discord channel.  


## Environment configuration variables  
**DISCORD_WEBHOOK** - webhook, where to post alerts. For more details see: https://support.discordapp.com/hc/en-us/articles/228383668-Intro-to-Webhooks   
**DISCORD_NAME** - bot name at Discord. Default AlertManager.  

**.env**
```sh
DISCORD_WEBHOOK=https://discord.com/api/webhooks/XXXXXXXXXXXXXXXX/YYYYYYYYYYYYYYYYYYYYYYYYYYYY
DISCORD_NAME=<webhook name>
```

## Example Prometheus Alertmanager config:  

Example Alertmanager config:  

```yml
global:
  # The smarthost and SMTP sender used for mail notifications.
  smtp_smarthost: 'localhost:25'
  smtp_from: 'alertmanager@example.org'
  smtp_require_tls: false

# The root route on which each incoming alert enters.
route:
  group_by: ['alertname', 'cluster', 'service']
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 4h
  receiver: discord_webhook

receivers:
- name: 'discord_webhook'
  webhook_configs:
  - url: 'http://alert-discord:9095'
```

For more details see: https://prometheus.io/docs/alerting/configuration/  

## Example Docker-Compose:  

```yml
  alert-discord:
    build:
      context: https://github.com/101101/alertmanager-discord.git
      dockerfile: ./Dockerfile
    container_name: alert-discord
    hostname: alert-discord
    ports:
      - 9095:9095
    expose:
      - 9095
    restart: always
    environment:  # declared vars take precedence over env_file  
      - DISCORD_WEBHOOK=${DISCORD_WEBHOOK}
      - DISCORD_NAME=${DISCORD_NAME}
```

| Contact: |
| :---------: |
| **[Slack](https://101101workspace.slack.com/archives/D012ESWSXHQ "dsmith73 on 101101 workspace")**  / **[Discord](https://discord.gg/RmzVNzx)** |
| ![github.com/dsmith73](https://avatars1.githubusercontent.com/u/44279121?s=60&u=7a933a33b51505f9d6435eeffae1c8156a47dc77&v=4 "github.com/dsmith73") |
