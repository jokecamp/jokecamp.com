---
author: Joe Kampschmidt
comments: true
layout: post
slug: ssh-task-variables-with-bamboo-build-server
title: Using variables with the SSH Task on Bamboo Build Server
desc: 'A quick example of how to use variable substitution with the Atlassian Bamboo SSH Task for build plans and deployment projects.'
tags:
- bamboo
- devops
---

**The quick solution is you can use `${bamboo.env}`, where env is your case-sensitive var, in any Task field (except the password).**

-----------

## Example Usage with Screenshots

The Atlassian [SSH Task][0] and [Bamboo Variables][1] docs do not give actual variable usage examples so I wanted to share a basic example with helpful screenshots. Below are some key features of Bamboo vars:

- Variable names are case sensitive
- You DO include the `bamboo` prefix for `${bamboo.env}` when referencing but not when defining.
- The `${bamboo.env}` syntax works on both windows and unix build agents
- Variables can be used with build plans and deployments.
- Variables can be used in all fields of a Task, with the exception of password variables.

I used this in a deployment project to deploy to an environment named `DT`.

### 1) Setup Deployment and Global variables

I have defined the following variables in Bamboo. I suggest using all *lowercase* since the variables are case-sensitive.

- A global variable called `unix_user` = `joe`
- A deployment variable called `env` = `dt`
- A deployment variable called `web_server_ip` = `a real ip address or hostname`

### 2) Configure the SSH Task

Inside the the ssh task for this deployment you can now use `${bamboo.env}`. In this example I am using variables two different ways.

1. As replacements for the `Host` and `Username` fields.
2. Inside the actual `SSH Command`. In this example I echo `${bamboo.deploy.release}` (deploy.release is a [built in bamboo deployment var][1]) for the logs and then I use my `${bamboo.env}` as a parameter to my custom deployment script that exists on the targeted server.

They are referenced with the same syntax. **It does not matter that my build server is windows and my targeted host is unix.**

<img src="{{ site.url }}/public/bamboo-ssh-task.png" alt="SSH Task Configuration Example" class="screenshot" style="width:60%;">

### 3) Verifying the substitutions in the logs

Bamboo will log each substitution. Below is what to look for in the deployment logs.

```
Substituting variable: ${bamboo.env} with dt
```

If you are watching the live/dynamic logs as it runs you will see an even more obvious notification like the one below:

<img src="{{ site.url }}/public/bamboo-log.png" alt="SSH Task Configuration Example" class="screenshot" style="width:80%;">


[0]:https://confluence.atlassian.com/bamboo059/using-the-ssh-task-in-bamboo-800858088.html
[1]:https://confluence.atlassian.com/display/BAMBOO055/Bamboo+variables
