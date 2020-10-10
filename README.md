<p align="center">
<img src="https://ashallendesign.co.uk/images/custom/laravel-config-validator-logo.png" width="500">
</p>

## Table of Contents

- [Overview](#overview)
- [Installation](#installation)
    - [Requirements](#requirements)
    - [Install the Package](#install-the-package)
- [Usage](#usage)
    - [Creating a Validation Ruleset](#creating-a-validation-ruleset)
        - [Ruleset Location](#ruleset-location)
        - [Adding Rules to a RuleSet](#adding-rules-to-a-ruleset)
        - [Custom Validation Error Messages](#custom-validation-error-messages)
    - [Running the Validation](#running-the-validation)
        - [Running the Validation Manually](#running-the-validation-manually)
        - [Using the Command](#using-the-command)
    - [Facade](#facade)
- [Security](#security)
- [Contribution](#contribution)
- [License](#license)

## Overview

A Laravel package that allows you to validate your config values and environment.

## Installation

### Requirements
The package has been developed and tested to work with the following minimum requirements:

- PHP 7.2
- Laravel 6

### Install the Package
You can install the package via Composer:

```bash
composer require ashallendesign/laravel-config-validator
```

## Usage

### Creating a Validation Ruleset

#### Ruleset Location

To validate your application's config, you need to define the validation rules first. You can do this by placing them inside
files in a ``` config/validation ``` folder with names that match the config file you're validating. As an example, to
validate the ``` config/app.php ``` config file, you would create a new file at ``` config/validation/app.php ``` that
would hold the rules.

#### Adding Rules to a Ruleset

Once you have your ruleset file created in the ``` config/validation ``` folder, you can start adding your validation
rules.

Under the hood, Laravel Config Validator uses the built-in ``` Validator ``` class, so it should seem pretty familiar
to work with. To check out the available Laravel validation rules that can be used, [click here](https://laravel.com/docs/8.x/validation#available-validation-rules).

As an example, we might want to add a config validation rule to ensure that the ``` driver ``` field in the ``` app/mail.php ```
file is a supported field. To do this, we could create a file at ``` config/validation/mail.php ``` with the following:

```php
<?php

use AshAllenDesign\ConfigValidator\App\Services\Rule;

return [
    Rule::make('driver')->rules(['in:smtp,sendmail,mailgun,ses,postmark,log,array']),
    // ...
];
```

#### Custom Validation Error Messages

There may be times when you want to override the error message for a specific validation rule. This can be done by passing
in an array containing the messages to the ``` ->messages() ``` method for a ``` Rule ```. This array should follow the same
pattern that would be used in a standard Laravel Validator object.

As an example, we might want to add a config validation rule to ensure that the ``` driver ``` field in the ``` app/mail.php ```
file is a supported field and also use a custom error message. To do this, we could update our validation file to the following:

```php
<?php

use AshAllenDesign\ConfigValidator\App\Services\Rule;

return [
    Rule::make('driver')
        ->rules(['in:smtp,sendmail,mailgun,ses,postmark,log,array'])
        ->messages(['in' => 'The mail driver is invalid']),
    // ...
];
```

### Running the Validation

#### Running the Validation Manually

To run the config validation you can call the ``` ->run() ``` method on a ``` ConfigValidator ``` object. The example below
shows how you could do this in a controller:

```php
<?php
    
namespace App\Http\Controllers;

use AshAllenDesign\ConfigValidator\App\Services\ConfigValidator;

class TestController extends Controller
{
    public function index()
    {
        $configValidator = new ConfigValidator();

        $configValidator->run();

        return response()->json(['success' => true]);
    }
}
```

If you aren't storing your validation files in the default ``` config/validation ``` folder, you can pass a custom folder path
into the ``` ->run() ``` method. As an example, if you had the files stored in a ``` app/Custom/Validation ``` folder, you
could use the following:

```php
$configValidator = new ConfigValidator();

$configValidator->run('app/Custom/Validation');
```

#### Using the Command

The library comes with a useful command that you can use to validate your config. To use it, you can run the following in
the command line:

```bash
php artisan config:validate
```

If you aren't storing your validation files in the default ``` config/validation ``` folder, you can pass a custom folder path
into the ``` --path ``` option. As an example, if you had the files stored in a ``` app/Custom/Validation ``` folder, you
could use the following:

```bash
php artisan config:validate --path=app/Custom/Validation
```

### Facade

If you prefer to use facades in Laravel, you can choose to use the provided ``` ConfigValidator ``` facade instead of instantiating the ``` AshAllenDesign\ConfigValidator\Classes\ConfigValidator ```
class manually.

The example below shows an example of how you could use the facade to run the config validation:

```php
<?php
    
namespace App\Http\Controllers;

use ConfigValidator;

class TestController extends Controller
{
    public function index()
    {
        ConfigValidator::run();

        return response()->json(['success' => true]);
    }
}
```

## Security

If you find any security related issues, please contact me directly at [mail@ashallendesign.co.uk](mailto:mail@ashallendesign.co.uk) to report it.

## Contribution

If you wish to make any changes or improvements to the package, feel free to make a pull request.

Note: A contribution guide will be added soon.

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.