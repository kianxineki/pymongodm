# pymongodm

pymongodm is a odm respecting pymongo functionality and adds functionality such as model validation.

change your current orm by pymongodb.

Before:

![Imgur](http://i.imgur.com/8TTqJ9h.jpg)


After:

![Imgur](http://i.imgur.com/NDna9Wp.jpg)

https://pypi.python.org/pypi/pymongodm

## install
```
pip install pymongodm
```


## connect db
 ```python
 import pymongodm
 
 
 pymongodm.connect('name_db')
 ```
 
or

```python
 import pymongodm
 import pymongo
 
 db = pymongo.MongoClient()['name_db']
 pymongodm.connect(db)
 ```
 
## use db
 ```python
 import pymongodm
 
 pymongodm.connect('example_db')
 
 # Identical to pymongo
 pymongodm.db.nice_collection.insert({'name': 'pepis'})
 print(pymongodm.db.nice_collection.find_one())
 ```
 
## use models!
 
 ```python
 import pymongodm
pymongodm.connect("gstudio")

from pymongodm.models import Base


class User(Base):
    schema = {"name": {'type': str},
              "other": {'type': list, 'required': False}}
    # optional, default is class_name + s
    collection_name = "random_name"

    def cut_name(self):
        return self.name[:3]

# insert
result = User({'name': 'pepito'})
print("id in db", result._id)

# convert dict to object Model

a = User({'_id': result._id, 'name': result.name})
b = User(result.getattrs())  # get attrs return only db attrs
b = User(result.get_clean())  # not return except arguments (exclude_view )

# convert result finds to model
results = pymongodm.db.users.find().model(User)
# or
results = User.collect.find().model(User)

for result in results:
    print(result._id)
    print(result.name)
    print(result.cut_name())


# Modify values
results = pymongodm.db.users.find().model(User)
for result in results:
    result.name = "Pymongodm_%s" % result.name
    result.other = ["random", "info"]
    result.update()

# Remove
result.remove()

 ```
 
 
### Models options
#### use other collection
by default the name of the collection is the name of the class + s.
It can be changed with the following argument:
collection = "encoding_profiles"

#### hidden arguments in returns
exclude_view = ['name']

#### schemes allow the following parameters:
 - type
 - required
 - function


## Rewrite basic methods
 Only need declare identic name in your class
 
```python
 
class User(Base):
    schema = {"name": {'type': str},
              "other": {'type': list, 'required': False}}
    # optional, default is class_name + s
    collection_name = "random_name"

    def remove(self):
        print("uhm ...")
```

## Plugins
TODO ...
