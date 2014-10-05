#Basebox
A vagrant setup including Docker, deployment tools (Fabalicous) optimized for nice and easy Drupal development. With this setup you will get a local development machine which is docker based + deployment tools to fetch all sites data from remote instances.

1. Fetch this repo and it's submodules
2. Follow the Fabalicous installation instructions: https://github.com/stmh/fabalicious (installing Fabric, pyyaml)
3. Copy fabfile.default.yaml to fabfile.yaml and put your project info. If you wanna just create a local version and be up and running quickly. Still it's highly recommended to have a look at the fabfile.default.yaml for all the options

```
# Example config for Drupal 8 (would work for 7 too). You need to have drupal downloaded to 'public'
name: drupal8
hosts:
  local:
    host: d8.dev
    user: root
    password: root
    port: 222
    rootFolder: /var/www/public
    gitRootFolder: /var/www
    siteFolder: /sites/default
    gitRootFolder: /var/www
    # path to the site's folder
    filesFolder: /sites/default/files
    backupFolder: /var/www/backups
    useForDevelopment: true
    branch: develop
    supportsInstalls: true
    hasDrush: true
    database:
      name: drupal
      user: root
      pass: admin```


4. [SSH agent forwarding must be enabled if you want to use all features of fabalicous] (https://developer.github.com/guides/using-ssh-agent-forwarding/).
5. `$ vagrant plugin install hostmanager`
6. `$ vagrant up`
