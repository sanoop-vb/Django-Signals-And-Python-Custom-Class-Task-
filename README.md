  
# Questions for Django Trainee at Accuknox

## Topic: Django Signals

#### Question 1: By default are django signals executed synchronously or asynchronously? Please support your answer with a code snippet that conclusively proves your stance. The code does not need to be elegant and production ready, we just need to understand your logic.

```Answer-
import time
from django.core.signals import request_finished
from django.dispatch import receiver

@receiver(request_finished)
def my_callback(sender, **kwargs):
    print("Signal received. Starting processing...")
    time.sleep(5)  
    print("Processing finished.")

print("Sending signal...")
request_finished.send(sender=None)
print("Signal sent.")
```

#### Question 2: Do django signals run in the same thread as the caller? Please support your answer with a code snippet that conclusively proves your stance. The code does not need to be elegant and production ready, we just need to understand your logic.

```Answer-
import threading
from django.db.models.signals import post_save
from django.dispatch import receiver
from django.db import models


class MyModel(models.Model):
    name = models.CharField(max_length=100)


@receiver(post_save, sender=MyModel)
def my_signal_handler(sender, instance, **kwargs):
    print(f"Signal received for: {instance.name}")
    print(f"Signal handler thread ID: {threading.get_ident()}")

    
obj = MyModel.objects.create(name="Test")
print("Object created")
print(f"Main thread ID: {threading.get_ident()}")
```
#### Question 3: By default do django signals run in the same database transaction as the caller? Please support your answer with a code snippet that conclusively proves your stance. The code does not need to be elegant and production ready, we just need to understand your logic.

```Answer-
from django.db import models, transaction
from django.db.models.signals import post_save
from django.dispatch import receiver


class MyModel(models.Model):
    name = models.CharField(max_length=100)


@receiver(post_save, sender=MyModel)
def my_signal_handler(sender, instance, **kwargs):
    print("Signal received. Inside transaction:", transaction.get_connection().in_atomic_block)
```
#### Topic: Custom Classes in Python

#### Description: You are tasked with creating a Rectangle class with the following requirements:

1.An instance of the Rectangle class requires length:int and width:int to be initialized

2.We can iterate over an instance of the Rectangle class 

3.When an instance of the Rectangle class is iterated over, we first get its length in the format: {'length': <VALUE_OF_LENGTH>} followed by the width {width: <VALUE_OF_WIDTH>}

```
class Rectangle:
    def __init__(self, length, width):
        self.length = length
        self.width = width

    def __iter__(self):
        yield {"length": self.length}
        yield {"width": self.width}

rect = Rectangle(7, 4)

for dimension in rect:
    print(dimension)
```
