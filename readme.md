## Lesson 28 OOP practices

[текст задания](the_task.jpg)

#### Создать класс для пользователя 

```python

class Users:
    pass

```

#### В котором будут такие атрибуты

    first_name -- имя пользователя
     
    last_name -- фамилия пользователя
    
    password -- пароль
    
    addresses -- адреса
    
    phones -- телефоны
    
    
```python

def __init__(self, first_name=None,
             last_name=None,
             password=None,
             addresses=None,
             phones=None):
    self.__salt = None
    self.first_name = first_name
    self.last_name = last_name
    self._password = self._set_password(password)
    self.addresses = self._check_addresses(addresses)
    self.phones = self._check_phones(phones)


```

#### Пароль должен быть зашифрован  

```python

import bcrypt

def _set_password(self, passwd):
    """создает хешированный пароль"""
    self.__salt = bcrypt.gensalt()
    return bcrypt.hashpw(passwd.encode(), self.__salt)

```

#### Пароль не должен быть доступен при помощи Users()._password

```python

class AccessDeniedException(Exception):
    def __init__(self, text, *args, **kwargs):
        print(text)

def __getattribute__(self, name):
    if name == "_password":
        raise (AccessDeniedException("в доступе отказано"))
    return super(Users, self).__getattribute__(name)

```

#### Реализовать сравнение пароля с произвольной строкой

```python

def check_password(self, input_str: str) -> bool:
    """ проверяет строку на совпадение с паролем """
    check_str = bcrypt.hashpw(input_str.encode(), self.__salt)
    return check_str == self.__dict__["_password"] if True else False

```  

#### телефонов не должно быть более 3 и длинна телефона не должно быть более 15 символов

```python

class CustomException(Exception):
    def __init__(self, text, *args, **kwargs):
        print(text)

def _check_phones(self, phones_list) -> list:
    """ проверяет количество телефонов (< 4) и количество символов ( < 16) """
    if self._check_phones_count(phones_list):
        if self._check_phones_length(phones_list):
            return phones_list
        raise CustomException("неправильный телефон")
    raise CustomException("много телефонов")

def _check_phones_count(self, phones_list):
    """ проверяет количество телефонов """
    return True if len(phones_list) < 4 else False

def _check_phones_length(self, phones_list):
    """ проверяет длинну телефонов """
    for phone in phones_list:
        if len(phone) > 15:
            raise CustomException("Неправильный телефон")
    return True

```

#### 



#### Проверить атрибут addresses на соответствие шаблону


```python

addresses = [{"country": "",
              "city": "",
              "billing_address": "",
              "index": ""}, ...
             ]

```

```python

def _check_addresses(self, addresses) -> dict:
    """ проверяет соответствие адреса шаблону """
    check_list = ["country", "city", "billing_address", "index"]
    for address in addresses:
        for item in address:
            if item not in check_list:
                raise CustomException("неправильный адрес")
    return addresses


```

