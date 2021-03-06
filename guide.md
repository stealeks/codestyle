# Using if
## Good

```php
if ($this->isAcceptable()) {
    return true;
}

return false;
```

## Bad
```php
if ($someCondition) return true;

if ($someCondition)
    return true;
    
if ($someVariable = $this->getSomeValue()) return $someVariable;
```

# Naming

## If function returns bool
Good | Bad
--- | ---
`isActive` | `getActive`
`isAdmin` | `getAdmin`

## Function name **must** explain what it does

Good | Bad
--- | ---
`getOneUserByLogin($login)`  | `getUser($string)`


## Variables

Good | Bad
--- | ---
`$isValid` | `$isvalid`
`$counter` | `$c`
`$tableRow` | `$tr`
`$countryLocalizationRepository` | `$countrylocalizationrepository`
`$destinationNumber` | `$number1`
`$callerNumber` | `$number2`

# Inheritance

## Global

### Bad
```php
/**
 * @return string
 */
public static function getRuHello()
{
    return 'Привет';
}

/**
 * @return string
 */
public static function getEnHello()
{
    return 'Hello';
}
```
### Good
```php

/**
 * @param string $locale
 * @return string
 */
public static function getHello($locale = self::LOCALE_RU)
{
    if ($locale == self::LOCALE_RU) {
        return 'Привет';
    }
    
    return 'Hello';
}
```
**or**
```php
/**
 * @param string $locale
 * @return string
 */
public static function getAlterHello($locale = self::LOCALE_RU)
{
    $translations = array(
        self::LOCALE_RU => 'Привет',
        self::LOCALE_EN => 'Hello'
    );
    
    if (isset($translations[$locale])) {
        return $translations[$locale];
    }
    
    return $locale;
}
```
### Very Good

**Parent**
```php
/**
 * @param array $translations
 * @param string $locale
 * @return string
 */
public static function getTranslation($translations, $locale = self::LOCALE_RU)
{
    if (isset($translations[$locale])) {
        return $translations[$locale];
    }
    
    return $locale;
}
```
**Child**
```php
/**
 * @param string $locale
 * @return string
 */
public static function getAlterHello($locale = self::LOCALE_RU)
{
    return self::getTranslation(array(
        self::LOCALE_RU => 'Привет',
        self::LOCALE_EN => 'Hello'
    ), $locale);
}
```


## Alignment

### Bad
```php
$userRepository     = $this->getUserRepository();
$clientRepository   = $this->getClientRepository();
$callRoomRepository = $this->getCallRoomRepository();
```
### Why?

If you need to add this line
```php
$faqRepository = $this->getFaqRepository();
```
It is easy
```php
$userRepository     = $this->getUserRepository();
$clientRepository   = $this->getClientRepository();
$callRoomRepository = $this->getCallRoomRepository();
$faqRepository = $this->getFaqRepository();
```
BUT, if you need to add THIS line
```php
$phoneNumberRepository = $this->getPhoneNumberRepository();
```
You will spend more time
```php
$userRepository     = $this->getUserRepository();
$clientRepository   = $this->getClientRepository();
$callRoomRepository = $this->getCallRoomRepository();
$faqRepository      = $this->getFaqRepository();
$phoneNumberRepository = $this->getPhoneNumberRepository();
```

## Assignment

### Bad
```php
$user=$this->getUser();
$user= $this->getUser();
$user =$this->getUser();
$elements = array('bad'=>'here it is','very_bad'=>'another one');
```
### Good
```php
$user = $this->getUser();
$lonely = array('can' => 'do it');
$elements = array(
    'bad' => 'here it is',
    'very_bad' => 'another one'
);
```

## Comparison

### Bad
```php
if ($currentUser==$owner) {
```
```php
if ($currentUser===$owner) {
```
```php
if ($currentUser!=$owner) {
```
### Good
```php
if ($currentUser == $owner) {
```
```php
if ($currentUser === $owner) {
```
```php
if ($currentUser != $owner) {
```

## Symfony

**Controller** must have method for each *service* and *repository*

**xxxRepository** - class which you **must** use to get entities from DB

**xxxManager** - class which you **must** use to create or edit entities

### Core Controller
```php
/**
 * @return UserManager
 */
public function getUserManager()
{
    return $this->get('acme.manager.user');
}

/**
 * @return ObjectRepository
 */
public function getRepostiory($entity, $namespace)
{
    return $this->getDoctrine()->getRepository($namespace.':'.$entity);
}
```
### Bundle Controller
```php
/**
 * @return ObjectRepository
 */
public function getRepostiory($entity, $namespace = 'AcmeDemoBundle')
{
    return parent::getRepository($entity, $namespace);
}

/**
 * @return UserRepository
 */
public function getUserRepostiory()
{
    return $this->getRepository('User');
}
```


## @PHPDoc
```php
/**
 * @param string $status
 * @return bool
 */
public function isItCold($status)
{
    return $status == 'cold';
}

/**
 * @param integer $id
 * @return User|null
 */
public function getById($id)
{
    return $this->find($id);
}

/**
 * @param array $data
 * @return array
 */
public function formatForApi(array $data)
{
    return array(
        'id' => $data['identifier'],
        'name' => $data['nickname']
    );
}

/**
 * @return User[]
 */
public function getNewUsers()
{
    return array(new User());
}
```
