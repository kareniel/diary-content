---
draft: true
title: Implementing continuous delivery of a front-end app on Bitbucket
author: jonathan
layout: post
date: 2016-10-27
url: /continuous-delivery-on-bitbucket/
categories:
  - tutorial
tags:
  - devops

---

# Implementing continuous delivery of a front-end app on Bitbucket

- A front-end app
- a remote Ubuntu machine
- A Bitbucket repo

## Create a 'deploy' user

list all users
```bash
λ cut -d : -f 1 /etc/passwd
```

create a deploy user
```bash
sudo adduser deploy
```

Make sure your deploy user has the rights to your `/var/www` folder

## Generate an SSH key

```bash
λ cd ~/.ssh
```

```bash
λ cp id_rsa id_rsa.bak
```

```bash
λ ssh-keygen -t rsa -C "your_email@example.com"
```

```bash
Generating public/private rsa key pair.
Enter file in which to save the key (/home/username/.ssh/id_rsa):
/home/username/.ssh/id_rsa already exists.
Overwrite (y/n)? y
```


## Create environment variables (& Add the private key to Bitbucket)

get a base64 encoded version of your private key
```bash
λ base64 < id_rsa
```

- ID_RSA:  base64 encoded version of your private key (make sure the secure checkbox is checked)
- DEV_DEPLOY_USER: the username of your deploy user (created earlier)
- DEV_SERVER: the ip address of your remote machine
- DEV_DIR: the directory where you want to upload the final project to


## Copy the public key to the server

```bash
λ ssh-copy-id -i id_rsa.pub deploy@127.0.0.1
```

## Create a known_hosts file

```bash
λ ssh-keyscan -t rsa 127.0.0.1 > my_known_hosts
```


## Write a deploy script

```yaml
image: node:4.6.0
pipelines:
  default:
    - step:
        script:
          - npm install
          - npm test
          - npm run build
          - mkdir -p ~/.ssh
          - cat my_known_hosts >> ~/.ssh/known_hosts
          - (umask 077; echo $ID_RSA | base64 --decode > ~/.ssh/id_rsa)
          - scp -r dist $DEV_DEPLOY_USER@$DEV_SERVER:$DEV_DIR
```

## Notes

- Make sure your tests return an error code


---

###### References

[1](https://answers.atlassian.com/questions/39243415/how-can-i-use-ssh-in-bitbucket-pipelines)
[2](https://answers.atlassian.com/questions/39429257/how-do-i-set-up-ssh-public-key-authentication-so-that-i-can-use-ssh-sftp-or-scp-from-my-bitbucket-pipelines-pipeline)
