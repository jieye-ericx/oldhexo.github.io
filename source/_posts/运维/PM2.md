---
toc: true
title: PM2
categories: 
 - 笔记
tags:
 - PM2
---

## 简介

对于线上项目，如果直接通过 node app 来启动，如果报错了可能直接停止导致整个服务崩溃，一般监控 node 有几种方案。
<!-- more -->
- supervisor: 一般用作开发环境的使用。
- forever: 管理多个站点，一般每个站点的访问量不大的情况，不需要监控。
- PM2: 一个进程管理工具，维护一个进程列表，可以用它来管理你的node进程，负责所有正在运行的进程，并查看node进程的状态，也支持性能监控，负载均衡等功能。

## PM2 的主要特性

- 内建负载均衡（使用 Node cluster 集群模块）
- 后台运行
- 0 秒停机重载，我理解大概意思是维护升级的时候不需要停机.
- 具有 Ubuntu 和 CentOS 的启动脚本
- 停止不稳定的进程（避免无限循环）
- 控制台检测
- 提供 HTTP API
- 远程控制和实时的接口 API ( Nodejs 模块,允许和 PM2 进程管理器交互 )

## 安装

```shell
// 全局安装pm2，依赖node和npm
npm install -g pm2
```

## 常用命令

### PM2 start

#### 启动一个node程序

```shell
pm2 start start.js
//Or start any other application easily:
$ pm2 start bashscript.sh
$ pm2 start python-app.py --watch
$ pm2 start binary-file -- --port 1520
```

![](https://cdn.jsdelivr.net/gh/radoapx/rad-figure-bed/PicGo/blogs/pm2/20200507163000)

#### 启动进程并指定应用的程序名

```shell
pm2 start app.js --name application1
```

#### 集群模式启动

```shell
// -i 表示 number-instances 实例数量
// max 表示 PM2将自动检测可用CPU的数量 可以自己指定数量
pm2 start start.js -i max
```

![](https://cdn.jsdelivr.net/gh/radoapx/rad-figure-bed/PicGo/blogs/pm2/20200507162950)

#### 添加程监视

```shell
// 在文件改变的时候会重新启动程序
pm2 start app.js --name start --watch
复制代码
```

![](https://cdn.jsdelivr.net/gh/radoapx/rad-figure-bed/PicGo/blogs/pm2/20200507162941)

#### 其他Options

```shell
# Specify an app name
--name <app_name>

# Watch and Restart app when files change
--watch

# Set memory threshold for app reload
--max-memory-restart <200MB>

# Specify log file
--log <log_path>

# Pass extra arguments to the script
-- arg1 arg2 arg3

# Delay between automatic restarts
--restart-delay <delay in ms>

# Prefix logs with time
--time

# Do not auto restart app
--no-autorestart

# Specify cron for forced restart
--cron <cron_pattern>

# Attach to application log
--no-daemon
```

### 列出所有进程

```shell
$ pm2 [list|ls|status]
```

![](https://cdn.jsdelivr.net/gh/radoapx/rad-figure-bed/PicGo/blogs/pm2/20200507154916.png)

### 重启、删除、停止、重新加载进程

```shell
$ pm2 restart app_name
$ pm2 reload app_name
$ pm2 stop app_name
$ pm2 delete app_name
```

除了使用`app_name`,还可以：

- `all` to act on all processes
- `id` to act on a specific process id

### 查看状态、日志、指标

#### 日志

To display logs in realtime:

```shell
$ pm2 logs
```

To dig in older logs:

```shell
$ pm2 logs --lines 200
```

#### 自适应监控面板

```shell
$ pm2 monit
```

![](https://cdn.jsdelivr.net/gh/radoapx/rad-figure-bed/PicGo/blogs/pm2/20200507155125.png)

还有个Web版

```shell
$ pm2 plus
```

![](https://cdn.jsdelivr.net/gh/radoapx/rad-figure-bed/PicGo/blogs/pm2/20200507155220.png)

### 查看某个进程具体情况

```shell
pm2 describe app
```

![img](https://user-gold-cdn.xitu.io/2018/8/26/16574b5b3d899dd4?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 设置pm2开机自启

开启启动设置，此处是CentOS系统，其他系统替换最后一个选项（可选项：ubuntu, centos, redhat, gentoo, systemd, darwin, amazon）

```shell
pm2 startup centos 
```

然后按照提示需要输入的命令进行输入

最后保存设置

```shell
pm2 save
```

### 官方推荐命令

```shell
# Fork mode
pm2 start app.js --name my-api # Name process

# Cluster mode
pm2 start app.js -i 0        # Will start maximum processes with LB depending on available CPUs
pm2 start app.js -i max      # Same as above, but deprecated.
pm2 scale app +3             # Scales `app` up by 3 workers
pm2 scale app 2              # Scales `app` up or down to 2 workers total

# Listing

pm2 list               # Display all processes status
pm2 jlist              # Print process list in raw JSON
pm2 prettylist         # Print process list in beautified JSON

pm2 describe 0         # Display all informations about a specific process

pm2 monit              # Monitor all processes

# Logs

pm2 logs [--raw]       # Display all processes logs in streaming
pm2 flush              # Empty all log files
pm2 reloadLogs         # Reload all logs

# Actions

pm2 stop all           # Stop all processes
pm2 restart all        # Restart all processes

pm2 reload all         # Will 0s downtime reload (for NETWORKED apps)

pm2 stop 0             # Stop specific process id
pm2 restart 0          # Restart specific process id

pm2 delete 0           # Will remove process from pm2 list
pm2 delete all         # Will remove all processes from pm2 list

# Misc

pm2 reset <process>    # Reset meta data (restarted time...)
pm2 updatePM2          # Update in memory pm2
pm2 ping               # Ensure pm2 daemon has been launched
pm2 sendSignal SIGUSR2 my-app # Send system signal to script
pm2 start app.js --no-daemon
pm2 start app.js --no-vizion
pm2 start app.js --no-autorestart
```

## 管理多个应用

您还可以创建一个名为生态系统文件的配置文件来管理多个应用程序。生成生态系统文件:

```shell
$ pm2 ecosystem
```

生成`ecosystem.config.js`文件：

```javascript
module.exports = {
  apps : [{
    name: "app",
    script: "./app.js",
    env: {
      NODE_ENV: "development",
    },
    env_production: {
      NODE_ENV: "production",
    }
  }, {
     name: 'worker',
     script: 'worker.js'
  }]
}
```

And start it easily:

```shell
$ pm2 start process.yml
```

Read more about application declaration [here](https://pm2.keymetrics.io/docs/usage/application-declaration/).

## 通过pm2配置文件来自动部署项目

[官网指南](https://pm2.keymetrics.io/docs/usage/deployment/)

### 首先是配置服务器与Github的ssh:

1. 在服务器中生成rsa公钥和私钥，当前是 **centos7** 下进行

2. 前提服务器要安装git，没有安装的先安装git，已安装的跳过

   ```
   yum –y install git
   ```

3. 生成秘钥

   ```
   ssh-keygen -t rsa -C "xxx@xxx.com"
   ```

   在~/.ssh目录下有 id_rsa和 id_rsa.pub两个文件，其中id_rsa.pub文件里存放的即是公钥key。

4. 登录到GitHub，点击右上方的头像，选择settings ，点击Add SSH key，把id_rsa.pub的内容复制到里面即可。

![](https://cdn.jsdelivr.net/gh/radoapx/rad-figure-bed/PicGo/blogs/pm2/20200507160716)

### 本地项目PM2配置文件

```shell
pm2 ecosystem
```

在项目跟目录下运行，会自动生成模板文件:

```json
{
  // Applications part
  "apps" : [{
    "name"      : "API",
    "script"    : "app.js",
    "env": {
      "COMMON_VARIABLE": "true"
    },
    // Environment variables injected when starting with --env production
    // http://pm2.keymetrics.io/docs/usage/application-declaration/#switching-to-different-environments
    "env_production" : {
      "NODE_ENV": "production"
    }
  },{
    "name"      : "WEB",
    "script"    : "web.js"
  }],
  // Deployment part
  // Here you describe each environment
  "deploy" : {
    "production" : {
      // 服务器的用户名
      "user" : "node",
      // Multi host is possible, just by passing IPs/hostname as an array
      "host" : ["212.83.163.1", "212.83.163.2", "212.83.163.3"],
      // 要拉取的git分支
      "ref"  : "origin/master",
      // Git repository to clone
      "repo" : "git@github.com:repo.git",
      // 拉取到服务器某个目录下
      "path" : "/var/www/production",
      // Can be used to give options in the format used in the configura-
      // tion file.  This is useful for specifying options for which there
      // is no separate command-line flag, see 'man ssh'
      // can be either a single string or an array of strings
      "ssh_options": "StrictHostKeyChecking=no",
      // To prepare the host by installing required software (eg: git)
      // even before the setup process starts
      // can be multiple commands separated by the character ";"
      // or path to a script on your local machine
      "pre-setup" : "apt-get install git",
      // Commands / path to a script on the host machine
      // This will be executed on the host after cloning the repository
      // eg: placing configurations in the shared dir etc
      "post-setup": "ls -la",
      // Commands to execute locally (on the same machine you deploy things)
      // Can be multiple commands separated by the character ";"
      "pre-deploy-local" : "echo 'This is a local executed command'"
      // Commands to be executed on the server after the repo has been cloned
      "post-deploy" : "npm install && pm2 startOrRestart ecosystem.json --env production"
      // Environment variables that must be injected in all applications on this env
      "env"  : {
        "NODE_ENV": "production"
      }
    },
    "staging" : {
      "user" : "node",
      "host" : "212.83.163.1",
      "ref"  : "origin/master",
      "repo" : "git@github.com:repo.git",
      "path" : "/var/www/development",
      "ssh_options": ["StrictHostKeyChecking=no", "PasswordAuthentication=no"],
      "post-deploy" : "pm2 startOrRestart ecosystem.json --env dev",
      "env"  : {
        "NODE_ENV": "staging"
      }
    }
  }
}
```

>  关于 `post-deploy` 
>
> you may have noticed the command `pm2 startOrRestart ecosystem.json --env production`. The `--env ` allows to inject different sets of environment variables.
>
> Read more [here](http://pm2.keymetrics.io/docs/usage/application-declaration/#switching-to-different-environments).

按照自己要求修改好后，就可以部署啦：

```shell
pm2 deploy <configuration_file> <environment> setup
```

如:(windows 记得使用 git bash 等unix命令行)

```shell
pm2 deploy ecosystem.json production setup # 这个命令将会在远程服务器上创建文件
```

### pm2 deploy

 `pm2 deploy help`:

```
pm2 deploy <configuration_file> <environment> <command>

  Commands:
    setup                run remote setup commands
    update               update deploy to the latest release
    revert [n]           revert to [n]th last deployment or 1
    curr[ent]            output current release commit
    prev[ious]           output previous release commit
    exec|run <cmd>       execute the given <cmd>
    list                 list previous deploy commits
    [ref]                deploy to [ref], the "ref" setting, or latest tag
```