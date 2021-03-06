PHP Structure Check
===================

[![Build Status](https://travis-ci.org/CubiclDev/php-structure-check.svg?branch=master)](https://travis-ci.org/CubiclDev/php-structure-check)
[![License](https://poser.pugx.org/cubicl/php-structure-check/license)](https://packagist.org/packages/cubicl/php-structure-check)

This library can check a complex array structure against a given requirement.
The purpose of this library is to create a better experience when testing a result set from an api or something similar.

## Installation

```
composer require cubicl/php-structure-check
```

## Usage

Create a requirement:
```php
$requirement = new ListType(
    new ObjectType([
        'foo' => new StringType(),
        'bar' => new IntType(),
        'buzz' => new AnyType(),
        'foobar' => new NullableType(new StringType()),
        'nested' => new ObjectType([
            'foobar' => new StringType(),
        ]),
    ])
);
```
Have some sort of external data you want to check.
```php
$data = [
    [
        'foo' => 'foe',
        'bar' => 'baz',
        'buzz' => 'foe',
        'foobar' => null,
        'nested' => [
            'foobar' => 'buzz',
        ],
    ], [
        'foo' => 'foe',
        'bar' => 7,
        'buzz' => 'foe',
        'foobar' => 'baz',
        'nested' => [
            'foobar' => 'bozz',
        ],
    ], [
        'foo' => [],
        'bar' => 9.1,
        'foobar' => 'baz',
        'nested' => [
            'foobar' => 'bazz',
        ],
    ],
];
```
Check the data against the requirement.
```php
$result = Checker::fulfills($data, $requirement);
```
The returned object holds information about the analysis. You can
check the result by calling `isValid()` on the result object. To
fetch the errors, simply call `getErrors`.

## Supported Types

Currently the following types are supported:

 * Any
 * Nullable
 * Bool
 * Numeric
 * Float
 * Int
 * String
 * Object
 * List
 * Datetime
 * Regex
 * Optional
 * Enum
 
There are some open issues with ideas for more types. Feel free to send pull requests.

Additionally you can implement the `TypeInterface` and use your own type implementations.
 
## Checks

Checks are special types which can be used to add more rules to a field. So you can check
the length of a string, the count of elements in an array or determine if
a numeric value is in a given range.
