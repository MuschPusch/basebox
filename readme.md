#Basebox
A vagrant setup including Docker, deployment tools (Fabalicous) optimized for nice and easy Drupal development. With this setup you will get a local development machine which is docker based + deployment tools to fetch all sites data from remote instances.

1. Fetch this repo and it's submodules
2. Follow the Fabalicous installation instructions: https://github.com/stmh/fabalicious (installing Fabric, pyyaml)
3. Copy fabfile.default.yaml to fabfile.yaml and put your project info. If you wanna just create a local version and be up and running quickly. Still it's highly recommended to have a look at the fabfile.default.yaml for all the options

```
name: PROJECTNAME
hosts:
  local:
    host: yourproject.dev
    user: root
    password: root
    port: 222
    useForDevelopment: true
    rootFolder: /var/www
    # path to the site's folder
    siteFolder: /sites/default
    filesFolder: /sites/default/files
    backupFolder: /var/www/backups
    useForDevelopment: true
    hasDrush: true
    supportsInstall: true

```


4. [SSH agent forwarding must be enabled if you want to use all features of fabalicous] (https://developer.github.com/guides/using-ssh-agent-forwarding/).
5. `$ vagrant plugin install hostmanager`
6. `$ vagrant up`
