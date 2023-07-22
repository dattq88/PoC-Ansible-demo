# Role: npm-ppa

## Before running task

- You can change nodejs version on group_vars/tag_Env_{{ env }} if you want

```bash
nodejs_setup_version: "13"
```

## After complete task

- Check nodejs version

```bash
$ node -v
v13.14.0
```

- Check npm version

```bash
$ npm -v
6.14.4
```
