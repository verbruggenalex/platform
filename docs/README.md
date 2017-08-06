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
  
### Docker Only
This requirement for docker only requires docker in docker support.
The configuration to accomplish this is complicated and if implemented
incorrectly can give you problems. We recommend this approach only
for seasond docker users.
<details open><summary><b>Required components <sup>(1)</sup></b></summary>

* **[Docker](https://docs.docker.com/engine/installation/linux/docker-ce/centos/)**
</details>

### Docker Plus
Instead of having the absolute minimal requirement you can install the
host level components Composer and Phing on the non-docker environment.
Then this can spin up the docker containers for you without having to
configure a complicated docker installation.
<details open><summary><b>Required components <sup>(3)</sup></b></summary>

* **[Composer](https://getcomposer.org/)**
* **[Phing](https://packagist.org/packages/phing/phing)**
* **[Docker](https://docs.docker.com/engine/installation/linux/docker-ce/centos/)**
</details>

### Docker Zero
If you are not interested in the advantages that the starterkit can give
you with the provided docker images you can keep a normal host only setup.
But it is very much recommended to use docker as it will give you
everything you need.
<details open><summary><b>Required components <sup>(2)</sup></b></summary>


* **[LAMP Stack](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-centos-7)**
* **[Composer](https://getcomposer.org/)**
</details>

## Installation

The build system for nexteuropa projects is packaged in a toolkit that can
be found here: [ec-europa/ssk](https://github.com/ec-europa/ssk). This is
the only required composer package to set up your project. If your project
is registered as a package as well you can use the composer create-project
command to complete installation in one single command:

```
$ composer create-project verbruggenalex/platform foldername dev-master
```

This command will clone the repository and run composer install on the project.
That command will itself call another composer install by the usage of the
composer hooks. This hook will install the toolkit at a separate location to
avoid any alterations to be made to the dependencies and/or build system.
Everything can be altered through your own extensions. You will be asked to
remove or keep the VCS files after checking out your project. For development
purposes you should NOT agree to remove these files. Only for purposes like
deployments this can be useful.

## Build properties
  
### build.properties.dist
This properties file contains the default settings and acts as a loading and
documentation file for the system to work correctly. Any time you install the
toolkit it will be copied to your repository root. Even though it is a template
you should not remove this file, but commmit it to your repository. The reason
for this is that it allows you to easily check the version of the toolkit and
what new properties were introduced or deprecated.
<details open><summary><b>Requirements <sup>(2)</sup></b></summary>

* **Never alter**
* **Always commit**
</details>

### build.properties.project

Always commit this file to your repository. This file is required for all
NextEuropa projects. Without it your build system will fail with a build
exception. It must contain a minimum set of properties, like project.id, etc.
A list of required properties is still to be delivered. Aside from the
required properties you can add any other properties that are project
specific and do not contain any credentials.
<details open><summary><b>Requirements <sup>(4)</sup></b></summary>

* **Always commit**
* **No credentials**
* **No environments**
* **Needed for builds**
</details>

### build.properties.local

This file will contain configuration which is unique to your development
environment. It is useful for specifying your database credentials and the
username and password of the Drupal admin user so they can be used during the
installation. Next to credentials you have many development settings that you
can change to your liking. Because these settings are personal they should
not be shared with the rest of the team. Make sure you never commit this file.
<details open><summary><b>Requirements <sup>(1)</sup></b></summary>

* **Never commit**
</details>

## Listing the available build commands

You can get a list of all the available Phing build commands ("targets") with a
short description of each target with the following command:

```
$ ./bin/phing
```

## Building a local development environment

```
$ ./bin/phing build-platform-dev
$ ./bin/phing install-platform
```

## Running Behat tests

The Behat test suite is located in the `tests/` folder. When the development
version is installed (by running `./bin/phing build-platform-dev`) the Behat
configuration files (`behat*.yml`) will be generated automatically using the
base URL that is defined in `build.properties.local`.

If you are not using the development build but one of the other builds
(`build-platform-dist` or `build-multisite-dist`) and you want to run the tests
then you'll need to set up the Behat configuration manually:

```
$ ./bin/phing setup-behat
```

In order to run JavaScript in your Behat tests, you must launch a PhantomJS
instance before. Use the `--debug` parameter to get more information. Please
be sure that the webdriver's port you specify corresponds to the one in your
Behat configuration (`behat.wd_host.url: "http://localhost:8643/wd/hub"`).

```
$ phantomjs --debug=true --webdriver=8643
```

If you prefer to use an actual browser with Selenium instead of PhantomJS, you
need to define the Selenium server URL and browser to use, for instance:

```
behat.wd_host.url = http://localhost:4444/wd/hub
behat.browser.name = chrome
```

The tests can also be run from the root of the repository (or any other folder)
by calling the behat executable directly and specifying the location of the
`behat.yml` configuration file.

Behat tests can be executed from the repository root by running:

```
$ ./bin/behat -c tests/behat.yml
```

With a single Phing task, you can run every tests suites:

```
./bin/phing behat
```

If you want to execute a single test, just provide the path to the test as an
argument. The tests are located in `tests/features/`. For example:

```
$ ./bin/behat -c tests/behat.yml tests/features/content_editing.feature
```

Some tests need to mimic external services that listen on particular ports, e.g.
the central server of the Integration Layer. If you already have services running
on the same ports, they will conflict. You will need to change the ports used in
build.properties.local.

Remember to specify the right configuration file before running the tests.

## Running PHPUnit tests

Custom modules and features can be tested against a running platform installation
by using PHPUnit. When the development version is installed (by running
`./bin/phing build-platform-dev`) the PHPUnit configuration file `phpunit.xml`
will be generated automatically using configuration properties defined in
`build.properties.local`.

If you are not using the development build but one of the other builds
(`build-platform-dist` or `build-multisite-dist`) and you want to run PHPUnit tests
then you'll need to set up the PHPUnit configuration manually by running:

```
$ ./bin/phing setup-phpunit
```

Each custom module or feature can expose unit tests by executing the following steps:

- Add `registry_autoload[] = PSR-4` to `YOUR_MODULE.info`
- Create the following directory: `YOUR_MODULE/src/Tests`
- Add your test classes in the directory above

In order for test classes to be autoloaded they must follow the naming convention below:

- File name must end with `Test.php`
- Class name and file name must be identical
- Class namespace must be set to `namespace Drupal\YOUR_MODULE\Tests;`
- Class must extend `Drupal\nexteuropa\Unit\AbstractUnitTest`

The following is a good example of a valid unit test class:

```php
<?php

/**
 * @file
 * Contains \Drupal\nexteuropa_core\Tests\ExampleTest.
 */

namespace Drupal\nexteuropa_core\Tests;

use Drupal\nexteuropa\Unit\AbstractUnitTest;

/**
 * Class ExampleTest.
 *
 * @package Drupal\nexteuropa_core\Tests
 */
class ExampleTest extends AbstractUnitTest {
  ...
}
```

PHPUnit tests can be executed from the repository root by running:

```
$ ./bin/phpunit -c tests/phpunit.xml
```


## Checking for coding standards violations

When a development build is created by executing the 'build-platform-dev' Phing
target PHP CodeSniffer will be set up and can be run with the following
command:

```
# Scan all files for coding standards violations.
$ ./bin/phpcs

# Scan only a single folder.
$ ./bin/phpcs profiles/common/modules/custom/ecas
```
