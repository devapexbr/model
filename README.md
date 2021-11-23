Model
=====

[![Build Status](http://img.shields.io/travis/devapexbr/model.svg)](https://travis-ci.org/devapexbr/model) [![Coverage Status](http://img.shields.io/coveralls/devapexbr/model.svg)](https://coveralls.io/r/devapexbr/model)

This model provides an Laravel eloquent-like base class that can be used to build custom models in Laravel or other frameworks.

Features
--------

 - Accessors and mutators
 - Model to Array and JSON conversion
 - Hidden attributes in Array/JSON conversion
 - Guarded and fillable attributes
 - Appending accessors and mutators to Array/JSON conversion
 - Attribute casting

You can read more about these features and the original Eloquent model on http://laravel.com/docs/eloquent

Installation
------------

Install using composer:

```bash
composer require devapexbr/model
```

Example
-------

```php

use DevApex\Model\GenericModel;

class User extends GenericModel {

    protected $hidden = ['password'];

    protected $guarded = ['password'];

    protected $casts = ['age' => 'integer'];

    public function save()
    {
        return API::post('/items', $this->attributes);
    }

    public function setBirthdayAttribute($value)
    {
        $this->attributes['birthday'] = strtotime($value);
    }

    public function getBirthdayAttribute($value)
    {
        return new DateTime("@$value");
    }

    public function getAgeAttribute($value)
    {
        return $this->birthday->diff(new DateTime('now'))->y;
    }
}

$item = new User(array('name' => 'john'));
$item->password = 'bar';

echo $item; // {"name":"john"}
```
