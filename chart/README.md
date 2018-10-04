
# Drupal

This Helm chart bootstraps a Drupal installation using an opinionated approach tailored for professional development. 
See the [Silta project](https://github.com/wunderio/silta) for more information on how we integrate this chart with CircleCI and other tools.  

## TL;DR

```bash
$ helm install --repo https://wunderio.github.io/charts/ drupal
```

## Configuration options

|      Parameter                |                     Description                     |                              Default                              |
|-------------------------------|-----------------------------------------------------|-------------------------------------------------------------------|
| `drupal.image`                | The docker image to be used.                        | This value must be specified when creating / updating a release |
| `drupal.postInstall`          | Shell commands to be executed after a new release is created. | `TODO`                                                 |
| `drupal.postUpgrade`          | Shell commands to be executed after a new release is upgrade. | `TODO`                                                 |
| `drupal.resources`            | The Kubernetes resources for the nginx container. | TODO `{cpu: {requests: 1m}` |
| `drupal.publicFiles.size`     | The size of the volume where Drupal public files are stored. | `1G`                                                 |
| `drupal.privateFiles.enabled` | Wether private files are present. | `false`                                                 |
| `drupal.publicFiles.size`     | The size of the volume where Drupal private files are stored. | `1G`                                                 |
| `drupal.cron.command`         | The shell command to be executed. This is executed in a separate container using image specified in `drupal.image`. |  `TODO`  |
| `drupal.cron.schedule`        | The cron schedule using the same [format](https://en.wikipedia.org/wiki/Cron) used by Kubernetes. |                                                  |
| `drupal.hashsalt`             | In case you need the Drupal hash salt to have a specific value, you can specify it here. |  random string |
| `drupal.env`                  | Environment variables can be added, where the key is the variable name. | `{}`                                                 |
| `nginx.image`                 | The docker image to be used for nginx. | This value must be specified when creating / updating a release |                      |
| `nginx.resources`             | The Kubernetes resources for the nginx container. | `{cpu: {requests: 1m}` |


The [MariaDB chart](https://github.com/helm/charts/tree/master/stable/mariadb) is used as a dependency, and 
configuration specific to MariaDB subchart can be added under the `mariadb` key. We provide the following default configuration:

```
mariadb:
  master:
    persistence:
      storageClass: standard
  replication:
    enabled: false
  db:
    name: drupal
    user: drupal
```


## Differences with other Drupal Helm charts

This chart includes the best practices from years of Drupal development and 
hosting best practices at [Wunder](https://wunder.io). It differs from the 
"stable" chart in the following ways:

- We use [Drush](https://www.drush.org/), for deployment commands and cron.
- Assumes a development model where all configuration changes are in version control.
- Uses nginx and php-fpm rather than Apache.