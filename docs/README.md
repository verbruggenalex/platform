[![Latest Stable Version](https://poser.pugx.org/drush/drush/v/stable.png)](https://packagist.org/packages/drush/drush) [![Total Downloads](https://poser.pugx.org/drush/drush/downloads.png)](https://packagist.org/packages/drush/drush) [![Latest Unstable Version](https://poser.pugx.org/drush/drush/v/unstable.png)](https://packagist.org/packages/drush/drush) [![License](https://poser.pugx.org/drush/drush/license.png)](https://packagist.org/packages/drush/drush) [![Documentation Status](https://readthedocs.org/projects/drush/badge/?version=master)](https://readthedocs.org/projects/drush/?badge=master)
Shields to consider: https://shields.io/

Note: This documentation is in progress and should not be relied on. The project is in full development.

# NextEuropa
The Next EUROPA IT Platform is the technical side of the digital
transformation programme at the Commission. The aim is to give the
Commission the modern platform it needs to make the most of new
digital trends. And to help the organisation build a public web
presence which is more relevant, coherent and cost-effective. This
composer project contains the Drupal profile which is used to
build the projects.

## Requirements
There are three separate ways of using the NextEuropa project. Either
you use an environment with Docker installed, an environment without.
Or a mix of both.
  
<details><summary><b>Docker Only</b></summary>

This requirement for docker only requires docker in docker support.
The configuration to accomplish this is complicated and if implemented
incorrectly can give you problems. We recommend this approach only
for seasond docker users.<br>*Required components*:
[Docker](https://docs.docker.com/engine/installation/linux/docker-ce/centos/)
</details>
<details><summary><b>Docker Plus</b></summary>

Instead of having the absolute minimal requirement you can install the
host level components Composer and Phing on the non-docker environment.
Then this can spin up the docker containers for you without having to
configure a complicated docker installation.<br>*Required components*:
[Composer](https://getcomposer.org/),
[Phing](https://packagist.org/packages/phing/phing),
[Docker](https://docs.docker.com/engine/installation/linux/docker-ce/centos/)
</details>
<details><summary><b>Docker Zero</b></summary>

If you are not interested in the advantages that the starterkit can give
you with the provided docker images you can keep a normal host only setup.
But it is very much recommended to use docker as it will give you
everything you need.<br>*Required components*:
[Composer](https://getcomposer.org/),
[LAMP Stack](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-centos-7)
</details>

## Installation
The build system for nexteuropa projects is packaged in a toolkit that can
be found here: [ec-europa/ssk](https://github.com/ec-europa/ssk). This is
the only required composer package to set up your project. If your project
is registered as a package as well you can use the composer create-project
command to complete installation in one single command:

<details><summary><b>composer create-project verbruggenalex/platform foldername dev-master</b></summary>

This command will clone the repository and run composer install on the project.
That command will itself call another composer install by the usage of the
composer hooks. This hook will install the toolkit at a separate location to
avoid any alterations to be made to the dependencies and/or build system.
Everything can be altered through your own extensions. You will be asked to
remove or keep the VCS files after checking out your project. For development
purposes you should NOT agree to remove these files. Only for purposes like
deployments this can be useful.
</details>

## Build properties
There are 3 different sets of build properties file that you can use. If you
are unfamiliar with the purpose behind each different type of properties file
please open the descriptions and read what they are designed for.

<details><summary><b>build.properties.local</b><br>
  <sup>
    <code>never commit</code>
  </sup>
</summary>

This file will contain configuration which is unique to your development
environment. It is useful for specifying your database credentials and the
username and password of the Drupal admin user so they can be used during the
installation. Next to credentials you have many development settings that you
can change to your liking. Because these settings are personal they should
not be shared with the rest of the team. Make sure you never commit this file.
</details>
<details><summary>
    <b>build.properties.dist</b><br>
    <sup>
      <code>never alter</code> 
      <code>always commit</code>
    </sup>
  </summary>

This properties file contains the default settings and acts as a loading and
documentation file for the system to work correctly. Any time you install the
toolkit it will be copied to your repository root. Even though it is a template
you should not remove this file, but commmit it to your repository. The reason
for this is that it allows you to easily check the version of the toolkit and
what new properties were introduced or deprecated.
</details>
<details><summary><b>build.properties</b><br>
  <sup>
    <code>always commit</code> 
    <code>no credentials</code><br>
    <code>no environments</code> 
    <code>needed for builds</code>
  </sup>
</summary>

Always commit this file to your repository. This file is required for all
NextEuropa projects. Without it your build system will fail with a build
exception. It must contain a minimum set of properties, like project.id, etc.
A list of required properties is still to be delivered. Aside from the
required properties you can add any other properties that are project
specific and do not contain any credentials.
</details>

## Listing the available build commands

You can get a list of all the available Phing build commands ("targets") with a
short description of each target with the following command:

```
$ ./bin/phing
```
