# refactoring-legacy-php
Practices for refactoring (PHP) legacy apps

## Set up environment

Check if there is any "Install / Setup" documentation. If exists, try to follow and report and/or fix any issue you found in documentation or code

## Strategy

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
