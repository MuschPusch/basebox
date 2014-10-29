#Basebox
A vagrant setup including Docker, Behat testing and deployment tools (Fabalicous) optimized for nice and easy Drupal development. With this setup you will get a local development machine which is docker based + deployment tools to fetch all sites data from remote instances.

To give some examples:

# Examples

`fab config copyFrom:staging.yourproject.com` # will fetch you the database and files from a remote instance to local

But also:

`fab config:live.yourproject.com copyFrom:staging.yourproject.com # will fetch you the database and files from a remote instance to your staging environment`

`fab config deploy:staging.yourproject.com # will fetch you the database and files from a remote instance to local`

`fab config:live.yourproject.com deploy # will run multiple commands on live: pull from git, revert features, clear cache etc.`

`fab config behat # will run your behat tests in your local docker instance`

# Install
1. Fetch this repo and it's submodules
2. Follow the Fabalicous installation instructions: https://github.com/stmh/fabalicious (installing Fabric, pyyaml)
3. Copy fabfile.default.yaml to fabfile.yaml and put your project info.

If you wanna just create a local version and be up and running quickly use a minimum fabfile.yaml like below. Still it's highly recommended to have a look at the fabfile.default.yaml for all the options

```
# Example config for Drupal 7 (will work for Drupal 8 too). You need to have drupal downloaded to 'public' (or adapt rootFolder accordingly)
name: drupal7
hosts:
  local:
    host: d7.dev
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
    vagrant:
      ip: <ip-address>
      localHttpPort: <local http port, optional>
    database:
      name: drupal
      user: root
      pass: admin
    docker:
      name: <docker-name>
```


4. [SSH agent forwarding must be enabled if you want to use advanced features of fabalicous e.g. copyFrom remote Docker instances] (https://developer.github.com/guides/using-ssh-agent-forwarding/).
5. `$ vagrant plugin install hostmanager vagrant-fabric`
6. `$ vagrant up`

# Behat integration
1. Run `composer install` for installing behat. (This will take a while)
2. Add `behatPath: /var/www/vendor/bin/behat` to your fabfile.yaml file
3. To run your tests locally: `fab config behat`
