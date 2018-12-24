# refactoring-legacy-php
Practices for refactoring (PHP) legacy apps

## Set up environment

Check if there is any "Install / Setup" documentation. If exists, try to follow and report and/or fix any issue you found in documentation or code

## Strategy

- Test coverage
- Know the application
- Collect data

### Use Docker

Why:

- This avoid you to install all dependencies of the project in your host machine.
- Probably you are working with a newer version that is not supported. Downgrading versions or working with multiple versions in your host is a mesh.
- When the app is ready for newer versions the upgrade will be fast. 

If the legacy app has no support for newer version of PHP 

### Use Travis

Test PHP syntax against all PHP files:

```find . -name "*.php" -type f -exec php -l {} \;```

```ỳaml
language: php
php:
  - '5.4'
  - '5.5'
  - '5.6'
  - '7.0'
  - '7.1'
  - '7.2'
  - '7.3'
script: find . -name "*.php" -type f -exec php -l {} \;
```
### Setup composer

```composer init```

Specify a license and the PHP versions that you are supporting. If it is a legacy app, you should probably start supporting deprecated PHP versions.

```
{
    "name": "santakadev/refactoring-legacy-php",
    "license": "GPL-3.0",
    "authors": [],
    "require": {
        "php": "^5.4 || ^7.0"
    }
}
```
By now, do not replace commited libraries. They could have modifications with respect to their official distribution, and migrating now could be time consuming. Stick with this ultli you need to upgrade some of its dependencies. In that case, only migrate the target libraries.

If the app you are refactoring is a library add ```composer.lock``` to ```.gitignore``

### Install PHPUnit

### Acceptance testing


