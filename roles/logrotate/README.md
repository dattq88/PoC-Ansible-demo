# Role logrotate

## After complete task

- **Check logrotate status**

```bash
$ cat /var/lib/logrotate/status

  logrotate state -- version 2
  "/var/log/syslog" 2020-5-22-10:49:38
  "/var/log/nginx/base18.error.log" 2020-5-21-8:0:0
  "/var/log/nginx/base18.access.log" 2020-5-21-8:0:0
  "/usr/local/rails_app/ansible/shared/log/*.log" 2020-5-22-10:0:0
  "/var/log/fail2ban.log" 2020-5-22-10:49:57i
```
