# refactoring-legacy-php
Practices for refactoring (PHP) legacy apps

## Set up environment

Check if there is any "Install / Setup" documentation. If exists, try to follow and report and/or fix any issue you found in documentation or code

## Strategy

- Test coverage
- Know the application
- Collect data

### Setup git

Why:
- Track your changes

```git init```

### Use Docker

Why:
- Avoid installing all dependencies in your host machine.
- Avoid downgrading your PHP and other packages versions of your host.
- Avoid working with multiple PHP and other packages versions in your host.
- Future PHP local migraton will be faster.

### Use Travis

Why:
- Automatic CI
- Syntax check against all PHP versions

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

Why:
- Install development libraries
- Autoload
- Future migration of commited libraries

```composer init```

add  ```/vendor/``` to ```.gitignore```

If the app you are refactoring is a library add ```composer.lock``` to ```.gitignore``

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

Add composer to .travis.yml.

¿Is library?:
- Yes -> composer update
- No -> composer install

```yaml
before_script:
  - composer update
```

### Setup PHPUnit

Why:
- 

### Acceptance testing

Why:
- 

