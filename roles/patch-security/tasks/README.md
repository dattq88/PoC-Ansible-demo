# SSM AGENT SETTING LOGS

## 1. Which systems do this role apply

- Ubuntu 16.04/18.04(Default has amazon-ssm-agent service)
- Any Server/Instance running amazon-ssm-agent service installed by snap

## 2. What does this role do

- Remove all log levels of AWS SSM Agent logs from syslog exclude Start/Stop service log(system log of snap application)
- Just monitor AWS SSM Agent log through /var/log/amazon/ssm folder

## 3. How to check

- Check amazon-ssm-agent log in syslog

```
tail -f /var/log/syslog | grep amazon-ssm-agent
```

- Check amazon-ssm-agent log still streamming

```
sudo tail -f /var/log/amazon/ssm/amazon-ssm-agent.log
sudo cat /var/log/amazon/ssm/errors.log
```
