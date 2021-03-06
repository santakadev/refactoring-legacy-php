# Refactoring legacy PHP (DRATF)

In an ideal world Legacy Code would not exist. In an ideal world PHP language would not exist. But here we are in a real world working with PHP Legacy Code.

This are practices and techniques I've used for refactoring legacy (PHP) applications and libraries.

## Guide style

TODO

## Should I refactor?

TODO

Alternatives to no refactor:
- Leave it as it is
- Start over (another form of refactor)

## Set up environment

Check if there is any "Install / Setup" documentation. If exists, try to follow and report and/or fix any issue you found in documentation or code

## Types of refactoring

By scope:
- Full refactor
- Partial refactor

By ???:
- Steady refactor
- Start over

## Strategy

- Know the application
- Collect data
- Improve documentation
- Test coverage

Small steps toward better (not perfect) design.

## What should I start refactoring?

- Cost / Reward chart (It has not to be in terms of money)
- Core domain
- Nee feature request
- Bugfix

## New software arquitecture

If you are refactoring the hole application to a new software arquitecture, focus on:
- Breaking dependencies (Decoupling)
- Split in modules (if necessary)

Don't go too for at this point (microservices, CQRS, event sourcing, DDD, hexagonal) simply try to solve the bad design of the software that will let you on the future pivot to other arquitectures.

## Basic tools setup

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

### Use Travis (or any CI tool)

Why:
- Automatic CI
- Syntax check against all PHP versions

Test PHP syntax against all PHP files:

```find . -name "*.php" -type f -exec php -l {} \;```

```yaml
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

```json
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

## Code review

Why:
- Get knowlage about the code you are working on.
- Priorize refactoring tasks.

Make a list of all code smell that you found

```
- PHP and HTML code merged
- Too long files
- Too long functions
- Unmeaningful names
- Bad folder structure
```

### Manual code review

### Tools: phploc, pdepend, phpmd, phpcpd, phpcs, phpcs, phpmetrics, etc.

If it is a legacy app folder structure can be a mesh. Try not to modify it by now and instead use tools options to exclude 3rd party code.

```
|-php
   |- ctrl
   |- external -> 3rd party libraries
   |- inc
   |- lib
   |- utilities
```

```sh
phploc --exclude external php
pdepend --summary-xml=pdepend.xml --ignore=external php
phpcpd --exclude external php
phpmd --exclude external php html cleancode
phpmetrics --report-html="./report" --exclude=external php
```

### phploc

### Software arquitecture review

### Security holes

Do not make public not exploit any security hold you found.

### Setup PHPUnit

### Setup Codepection

### Setup PHPSpec

### Setup Behat

### Acceptance testing

### Stored procedures

- Create integration tests that covers all branches of stored procedure.
- Once is fully tested, comment the stored procedure call.
- Move and adapt stored procesure to PHP code.
- Refactor the code. Try to extract entities and value objects moving domain logic to them and create a repository for each entiry.
- Remove commented call.
- Drop stored procedure.

TODO: Procedurs unit test?
https://github.com/hepabolu/mytap

### PHP and HTML toguether

- Move al calculations to functions in the beggining of file
- Move al data to variables in the beggining of file

### Missing documentation

If you found something difficult to understand, but you can't change current implementation by now, add documentation to save time for the rest of developers.

### No convention

### No modules

You can start thinking about how you can slice the software in different modules.

### Handling UI/UX refactoring

UI refactoring -> better look with same functionalities
UX refactoring -> better user experience with same functionarlities

If you are working with a legacy app, the UI/UX will be probably outdated and buggy. 

### No routing

No routing sistem. Bunch of PHP scripts

```
index.php
login.php
profile.php
...
```

Try to make small steps towards a robuts routing system.

Examples:
- Create a *index_routed.php* file 

```
<?php

$routes = [
    '/'             => 'index.php',
    '/install'      => 'install.php',
    '/providers'    => 'manage_providers.php'
];

TODO: ¿parameters and query?
$uri = isset($_GET['q']) ? $_GET['q'] : '/';

if (!isset($routes[$uri])) {
    http_response_code(404);
    echo 'Not found';
    return;
}

$script = $routes[$uri];

require $routes[$uri];
```

- Use apache rewrite

- Use routing library

If necesary, create integration test to force new scripts to have a route.


### Refactoring tools

Check if your refactoring tools are working in all cases.

For example, when renaming *do_stored_query* the call_user_func_array is refactored by the tool?:

```php
function do_stored_query()
{
  ...
}

call_user_func_array('do_stored_query', $args)
```

What happens when changing signature?

### Constant based seam

### Collect and share funny comments

### Extract Domain concepts

- Comments
- Naming
- Persistence
- Documentation
- UI

### Test ouput buffer

```php
ob_start();
subject_under_test_call();
$ouput = ob_get_clean();
```
