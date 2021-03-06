# iban-validation

A small library for validating International Bankaccount Numbers (IBANs) based on the IBAN Registry provided by SWIFT.
See https://www.swift.com/taxonomy/term/3876 for more information.

[![Build Status](https://travis-ci.org/jschaedl/iban-validation.png)](https://travis-ci.org/jschaedl/iban-validation)
![PHP Version](https://img.shields.io/badge/version-PHP%207.1%2B-lightgrey.svg)

[![Total Downloads](https://poser.pugx.org/jschaedl/iban-validation/downloads)](https://packagist.org/packages/jschaedl/iban-validation) 
[![Latest Stable Version](https://poser.pugx.org/jschaedl/iban-validation/v/stable)](https://packagist.org/packages/jschaedl/iban-validation) 
[![Latest Unstable Version](https://poser.pugx.org/jschaedl/iban-validation/v/unstable)](https://packagist.org/packages/jschaedl/iban-validation) 
[![License](https://poser.pugx.org/jschaedl/iban-validation/license)](https://packagist.org/packages/jschaedl/iban-validation) 


## Development status

This library is ready to use. The Iban validation should be fine, but there is no warranty. **Please use it at your own risk.**

---

## Features

* full country support of IBAN validation based on SWIFT Registry
* customizable violation messages
* simple to use object oriented api
* high test coverage
* DIC friendly

---

## Installation

To install `jschaedl/iban-validation` via [composer](https://getcomposer.org/) use

```sh
$ composer require jschaedl/iban-validation
```

You can see this library on [Packagist](https://packagist.org/packages/jschaedl/iban-validation).

---

## Iban Validation

```php
<?php

use Iban\Validation\Validator;
use Iban\Validation\Iban;

$iban = new Iban('DE89 3704 0044 0532 0130 00');
$validator = new Validator();

if (!$validator->validate($iban)) {
    foreach ($validator->getViolations() as $violation) {
        echo $violation;
    }
}

```

You can also customize the violation messages by providing them via configuration. Just create a `Validator` passing a config array as constructor argument.

```php
<?php

use Iban\Validation\Validator;

$validator = new Validator([
    'violation.unsupported_country' => 'The requested country is not supported!',
    'violation.invalid_length' => 'The length of the given Iban is not valid!',
    'violation.invalid_country_code' => 'The country code of the given Iban is not valid!',
    'violation.invalid_format' => 'The format of the given Iban is not valid!',
    'violation.invalid_checksum' => 'The checksum of the given Iban is not valid!',
]);

```

## Iban Information

```php
<?php

use Iban\Validation\Iban;
use Iban\Validation\CountryInfo;

$iban = new Iban('IBAN DE89 3704 0044 0532 0130 00');
$iban->getCountryCode(); // 'DE'
$iban->getChecksum(); // '89'
$iban->getBban(); // '370400440532013000'
$iban->format(Iban::FORMAT_PRINT); // 'DE89 3704 0044 0532 0130 00'
$iban->format(Iban::FORMAT_ELECTRONIC); // 'DE89370400440532013000'

$countryInfo = new CountryInfo($iban->getCountryCode());
$countryInfo->getCountryName(); // 'Germany'
$countryInfo->getIbanStructureSwift(); // 'DE2!n8!n10!n'
$countryInfo->getBbanStructureSwift(); // '8!n10!n'
$countryInfo->getIbanRegex(); // '/^DE\d{2}\d{8}\d{10}$/'
$countryInfo->getBbanRegex(); // '/^\d{8}\d{10}$/'
$countryInfo->getIbanLength(); // 22
$countryInfo->getBbanLength(); // 18
$countryInfo->getIbanPrintExample(); // 'DE89 3704 0044 0532 0130 00'
$countryInfo->getIbanElectronicExample(); // 'DE89370400440532013000'

```

---
 
## How to contribute
If you want to fix some bugs or want to enhance some functionality, please fork the master branch and create your own development branch. 
Then fix the bug you found or add your enhancements and make a pull request. Please commit your changes in tiny steps and add a detailed description on every commit. 

### Unit Testing

All pull requests must be accompanied by passing unit tests. This repository uses phpunit and Composer. 
You must run `composer --dev install` to install this package's dependencies and `cp phpunit.xml.dist phpunit.xml` 
before the unit tests will run. You can run the test via:

```sh
$ vendor/bin/phpunit -c phpunit.xml Tests/
```

---
   
## Author

[Jan Schädlich](https://www.linkedin.com/in/janschaedlich)

## License

MIT License

---

## Release Status

2018-08-19: [Version 1.0](https://github.com/jschaedl/iban-validation/releases/tag/v1.0) has been released.
