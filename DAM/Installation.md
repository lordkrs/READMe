# DAM Installation

This guide will help you to install DAM Project in **PRODUCTION Mode**. Note this guide will mostly refer all paths as relative paths. 
>**Note** It is assumed that you have installed Ubuntu 20.04(Fresh Deployment), with no configurations done in the system related to python,nginx,mongo and npm.

## Prerequisites 

> Clone or Download the following repositories
   1. [AGcore](https://github.com/workflowrepo/agcore)
   2. [DAM](https://github.com/workflowrepo/dam)
   3. [DAMweb](https://github.com/workflowrepo/damweb)


## AGcore Installation
> Click [here](https://github.com/workflowrepo/agcore) to know more about AGcore

1. Install all the **AGcore** dependencies
```
$  cd agcore/install
$  ./install.sh
```
2. Deploy **Core** services and update the supervisor
```
$  cd agcore/install
$  sudo ./deploy.sh ./deployment_core_architecture.ini <system user>
```

## DAM Core Plugins Installation
> Do this after following installing [AGcore](#AGcore-Installation). Click [here](https://github.com/workflowrepo/dam) to know more about DAM.
```
$  cd dam/src
$  for i in $(ls -d */); do echo "Installing "${i%%/};agcorecl upgrade ${i%%/} -f --protos-path ./appcore/protos; done
```

## DAM Core Services Installation
> Do this after following installing [DAM Plugins](#DAM-Core-Plugins-Installation). Click [here](https://github.com/workflowrepo/dam) to know more about DAM.
```
$  cd dam/services/mountservice/install
$  sudo ./deploy.sh <system user>
$  cd dam/services/notifier/install
$  sudo ./install.sh <system user>
```

## Install Nginx(with Upload Module)
```
$  cd dam/install/submodules
$  ./install_nginx.sh
```

## DAMWeb Installation
### Install NodeJS

Install NodeJS using [Node Version Manager (nvm)](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04#option-3-%E2%80%94-installing-node-using-the-node-version-manager)
::: tip
NodeJS version >= v12.18.4 is recomended.
:::

### Install Vue

Follow steps below to install vue app.
#### Install vue cli

 ```bash
$  npm install -g @vue/cli
```
For further references follow the link [Vue.js installation](https://vuejs.org/v2/guide/installation.html)

### Install Dependencies

```bash
$  cd damweb
$  npm install
```

### Build DamWeb
```bash
$ npm run build
```

>Build **index.html** will be available inside **damweb/dist** folder serve this path in nginx for damweb, Update the location of dist folder in /etc/nginx/sites-enabled/damweb

```
$  sudo nginx -s reload 
$  sudo service supervisor stop
$  sudo service supervisor start
```
> DAMweb can be accessed using https://YOUR_IP, ex https://192.168.1.185