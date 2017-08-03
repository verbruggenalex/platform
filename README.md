# Subsite Starterkit Template

This is a simple template package for Composer that allows the ec-europa/ssk to
be installed with a single command. This template is only usefull as an example
and for new projects to set up their initial repository structure.

The command to be executed is:
```
composer create-project verbruggenalex/subsite foldername dev-master
```

This command will download the template repository and run the composer scripts
that are defined in the hooks. These hooks will make sure that you install the
latest starterkit. After which the starterkit will generate your repository
structure.

The structure you will end up with is the following:
```
drwxr-xr-x  6 root root 4096 Jun 20 22:56 .
drwxr-xr-x 17 root root 4096 Jun 20 22:55 ..
-rw-r--r--  1 root root  349 Jun 20 22:56 build.project.xml
-rw-r--r--  1 root root  297 Jun 20 22:56 build.properties
-rw-r--r--  1 root root  343 Jun 20 22:56 build.xml
-rw-r--r--  1 root root  448 Jun 20 22:55 composer.json
-rw-r--r--  1 root root 1601 Jun 20 22:55 composer.lock
-rw-r--r--  1 root root   22 Jun 20 22:55 .gitignore
-rwxr-xr-x  1 root root  265 Jun 20 22:56 Jenkinsfile
drwxr-xr-x  6 root root 4096 Jun 20 22:56 lib
-rw-r--r--  1 root root 1437 Jun 20 22:56 phpcs-ruleset.xml
-rw-r--r--  1 root root   30 Jun 20 22:55 README.md
drwxr-xr-x  3 root root 4096 Jun 20 22:56 resources
lrwxrwxrwx  1 root root   42 Jun 20 22:56 ssk -> vendor/ec-europa/ssk/includes/composer/bin
drwxr-xr-x  4 root root 4096 Jun 20 22:56 tests
drwxr-xr-x  4 root root 4096 Jun 20 22:55 vendor
```

The following files should not be edited as they are the connection to your
starterkit installation:
```
-rw-r--r--  1 root root  343 Jun 20 22:56 build.xml
-rw-r--r--  1 root root  448 Jun 20 22:55 composer.json
-rwxr-xr-x  1 root root  265 Jun 20 22:56 Jenkinsfile
```

The codebase for your project is generated with the contents of both lib/ and
resources/. Further information on how your project is built can be found in
the ec-europa/ssk repository.
