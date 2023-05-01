---
title: Linux(AWS)
category: Et Cetera
order: 3
---

### Shell of Linux

| Shell| stand for |
| ------------ |----------|
| bash | Bourne-Again Shell  |
|   sh | Bourne Shell  |
| csh | C Shell  |
| ksh | Korn Shell  |


### Authority

basically Authority will be shown partically(w 3 parts). 

(owner's Auth)+(group's Auth)+(extra user's Auth)

**1. How to discribe in Linux**
|auth|char|when it's file | when it's folder|
|---|---|---|---|
|read|r|read & copy|ls available|
|write|w|modification|file creation available|
|execution|x|execution|cd available|
| none | - |none|none|


**2. Example**
| Type    | Code |
| ------------ | ---- |
| rwxrwxrwx    | 777  |
| r-xr-rxr-x   | 555  |
| r---------  | 400  |
| rwx-------  | 700  |



### Basic Commands for Linux

| CMD| use to |
| ------------ |----------|
| **sudo** | get administrator authority to Command |
| **man** | see manual |
| **whoami** | know userId logged in right now  |
| **apt-get update** | program update(administrator Auth)  |
| **pwd** | know current directory  |
| **cd** | move to specific directory 
|       |**cd ..** > to previous dir  |
|        |**cd ~** > to home dir  |
|        |**cd /parentdir/childdir** > to /parentdir/childdir  |
| **ls** | list dir contents  |
|        |**ls -al** > list "all" dir |


### Access to Linux from MacOs

**1. Open your .pem in Terminal (at the file where pem is placed)**

```
chmod 400 bzhs1992.pem
```
**2. Enter ssh -i (pemName.pem) type Of Instance@elastic ip addr**

ssh = Secured Access
```
ssh -i bzhs1992.pem ubuntu@3.37.241.135
```
**3. type "yes" to Connect**
```
1.135
The authenticity of host '3.37.241.135 (3.37.241.135)' can't be established.
ED25519 key fingerprint is SHA256:dk6Puy97IkTH/oT6PtO3T31+Y2Goo6KdYJcxMRTxxtE.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
```