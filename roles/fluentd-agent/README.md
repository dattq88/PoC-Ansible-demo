# Ansible roles fluentd

## 1. Operation System

- Ubuntu 18.04
- Fluentd agent 4.0.1

- Orther version, change link download to roles/fluentd/tasks/main.yml line 4:

    ```bash
    url: 'curl -L https://toolbelt.treasuredata.com/sh/install-ubuntu-bionic-td-agent4.sh'
    ```

## 2. Plugin td-agent using

- fluent-plugin-s3: Push ec2-log to s3.
- fluent-plugin-ec2-metadata: Get instance_id ec2 use path prefix of the files on S3
- fluent-plugin-forest: Embed ec2-tag from fluent-plugin-ec2-metadata to path prefix on fluent-plugin-s3

## 3. Template configuration

- Template configuration example push syslog to s3:

- Input config

    ```bash
    <source>
        @type tail
        path /var/log/syslog # /path/to/source/log
        pos_file /var/log/td-agent/syslog.pos
        tag syslog
        <parse>
            @type syslog #format input
        </parse>
    </source>
    ```

- Notes: Fluentd needs permission to read the log file

- Output config

  ```bash
  <match syslog>                             # Match tag input config
    @type ec2_metadata                       # Using plugin ec2-metadata
    output_tag ${instance_id}.${tag}         # Output_tag example: i-0e3f95e438353e0ed.syslog
  </match>

  <match i-**.syslog>                        # Match output_tag @ec2_metadata
    @type forest                             # Output fluent-plugin-forest
    subtype s3                               # Sub output fluent-plugin-s3
    <template>
      <instance_profile_credentials>
        ip_address 169.254.169.254           # Default 169.254.169.254
        port 80                              # Default 80
      </instance_profile_credentials>

      s3_bucket {{ project }}-logs-{{ env }} # The Amazon S3 bucket name
      s3_region {{ region }}                 # The Amazon S3 region name. Default is us-east-1
      path ec2-logs/{{ type }}/${tag_parts[0]}/syslog/       # The path prefix of the files on S3.
      store_as gzip                          # Zip file push to S3
      s3_object_key_format %{path}%{time_slice}_%{index}.%{file_extension}
      time_slice_format %Y%m%d%H%M
      slow_flush_log_threshold 60s

      <buffer>
        timekey 60
        chunk_limit_size 8M
        queue_limit_length 32
        flush_mode interval
        flush_interval 5s
        overflow_action drop_oldest_chunk
        retry_forever true
        retry_max_interval 30
      </buffer>
    </template>
  </match>
  ```

## 4. Logs type

- **Bastion**
  - [x] auth.conf.j2
  - [x] syslog.conf.j2

- **Worker**
  - [x] auth.conf.j2
  - [x] syslog.conf.j2
  - [x] rails_app.conf.j2
  - [x] shoryuken.conf.j2
  - [x] codedeploy-agent.conf.j2
  - [ ] codedeploy-update.conf.j2
  - [x] codedeploy-deployments.conf.j2

- **API**
  - [x] auth.conf.j2
  - [x] syslog.conf.j2
  - [x] rails_app.conf.j2
  - [x] nginx-error.conf.j2
  - [x] nginx-access.conf.j2
  - [x] nginx-error-log.conf.j2
  - [x] nginx-access-log.conf.j2
  - [ ] codedeploy-agent.conf.j2
  - [x] codedeploy-deployments.conf.j2

- **Admin**
  - [x] auth.conf.j2
  - [x] syslog.conf.j2
  - [x] rails_app.conf.j2
  - [x] nginx-error.conf.j2
  - [x] nginx-access.conf.j2
  - [x] nginx-error-log.conf.j2
  - [x] nginx-access-log.conf.j2
  - [ ] codedeploy-agent.conf.j2
  - [x] codedeploy-deployments.conf.j2

- **Batch**
  - [x] auth.conf.j2
  - [x] syslog.conf.j2
  - [x] rails_app.conf.j2
  - [x] paygent_app.conf.j2
  - [x] codedeploy-agent.conf.j2
  - [x] codedeploy-deployments.conf.j2
  - [x] ssm-agent.conf.j2
  - [x] ssm-error.conf.j2

## 5. Using role fluentd

- Add role fluentd-agent in playbook:
  - [x] playbook-bastion.yml
  - [x] playbook-api.yml
  - [x] playbook-admin.yml
  - [x] playbook-batch.yml
  - [x] playbook-worker.yml
