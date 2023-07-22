# Role: Base

## Why Should Create a Deploy User

The main benefits of using a **dedicated deploy user are security and easier access management**. Security is benefited by removing the need to share root or other user access, essentially siloing the entire deployment process on the remote server. Access management is made easier (and more secure) because you can grant or restrict access by adding or removing users’ public SSH keys.

## Check PAM limits setting is correct

- Run the command

  ```ulimit -n```

## Check NTP service setting is correct

- **Check awscli version**

  ```aws --version```

  Output:

  ```bash
  aws-cli/1.18.80 Python/2.7.17 Linux/5.3.0-1023-aws botocore/1.17.3
  ```

- **Check status service ntp**

  ```service ntp status```

  Output:

  ```bash
  ntp.service - LSB: Start NTP daemon
     Loaded: loaded (/etc/init.d/ntp; bad; vendor preset: enabled)
     Active: active (running) since Fri 2020-04-17 14:02:36 +07; 5 days ago
     Docs: man:systemd-sysv-generator(8)
     Tasks: 1
     Memory: 1.6M
        CPU: 29.584s
     CGroup: /system.slice/ntp.service
              └─1169 /usr/sbin/ntpd -p /var/run/ntpd.pid -g -u 111:117
  ```

- **Check timezone on server**

  ```ls -l /etc/localtime```

  Output:

  ```bash
  lrwxrwxrwx 1 root root 32 Apr 20 16:40 /etc/localtime -> ../usr/share/zoneinfo/Asia/Tokyo```
  ```

- **Check datetimectl status**

  ```timedatectl status```

  Output:

  ```bash
                        Local time: Wed 2020-04-22 22:58:36 JST
                    Universal time: Wed 2020-04-22 13:58:36 UTC
                          RTC time: Wed 2020-04-22 13:58:37
                         Time zone: Asia/Tokyo (JST, +0900)
         System clock synchronized: yes
  systemd-timesyncd.service active: yes
                   RTC in local TZ: no
  ```
