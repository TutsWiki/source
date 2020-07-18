---
date: 2020-07-18
linktitle: Abstract classes and interfaces in Python
title: Abstract classes and interfaces in Python
weight: 10
url: /abstract-classes-and-interfaces-in-python
description: Abstract classes, methods and Zope interfaces, adapters in Python
keywords:
  - python
  - abs
  - abstract base class
  - interfaces
  - zope
  - adapters
  - programming
---
<meta property="og:image" content="https://tutswiki.com/img/tutswiki-logo.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="Abstract classes and interfaces in Python" />
<meta name=”twitter:description” content="Abstract classes, methods and Zope interfaces, adapters in Python" />

Abstract base classes and interfaces are entities that are similar in purpose and meaning. Both the first and second are a peculiar way of documenting the code and help to limit (decouple) the interaction of individual abstractions in the program (classes).

Python is a very flexible language. One facet of this flexibility is the possibilities provided by metaprogramming. And although abstract classes and interfaces are not represented in the core of the language, the former were implemented in the standard abc module, and the latter in the Zope project (the zope.interfaces module).

It makes no sense to use both at the same time, and therefore each programmer must determine for himself which tool to use when designing applications.

## Abstract base classes (abc)

Starting from version 2.6 of the language, the `abc` module is included in the standard library, which adds abstract base classes (hereinafter ABC) to the language.

ABC allows you to define a class, indicating which methods or properties must be overridden in inherited classes:

```python
from abc import ABCMeta, abstractmethod, abstractproperty
class Movable():
    __metaclass __ = ABCMeta

    @abstractmethod
    def move():
    """Move object"""
    
    @abstractproperty
    def speed():
    """Object speed"""
```

Thus, if we want to use an object with the ability to move and a certain speed in the code, then we should use the `Movable` class as one of the base classes.

The presence of the necessary methods and attributes of the object is now guaranteed by the presence of `abstractmethod` and `abstractproperty`.

```python
class Car(Movable):
    def __init__:
        self.speed = 10
        self.x = 0

    def move(self):
        self.c += self.speed
        def speed(self):
            return self.speed
    
assert issubclass (Car, Movable)
assert ininstance (Car(), Movable)
```

It can be seen that the concept of ABC fits well into the class inheritance hierarchy, it is easy to use them, and the implementation, if you look into the source code of the abc module, is very simple. Abstract classes are used in the standard collections and number modules, defining the methods of custom
inherited classes necessary for definition.

Details and considerations for using ABC can be found in [PEP-3119](http://www.python.org/dev/peps/pep-3119/).

## Interfaces (zope.interfaces)

The Zope Toolkit (ZTK) is a set of libraries intended for reuse by projects to develop web applications or web frameworks. It is developed by the contributors of the Zope Foundation. The zope framework has evolved into a set of almost independent components. The glue that holds the components together is the interfaces and the adapters based on them.

The zope.interfaces module is the result of this work.

In the simplest case, using interfaces is similar to ABC:

```python
import zope.interface

class IVehicle(zope.interface.Interface):
    """Any moving thing"""
    speed = zope.interface.Attribute("""Movement speed""")
    def move():
        """Make a single step"""
    
class Car(object):
    zope.interface.implements (IVehicle)

    def __init__:
        self.speed = 1
        self.location = 1

    def move (self):
        self.location = self.speed * 1
        print("moved!")
    
assert IVehicle.implementedBy (Car)
assert IVehicle.providedBy (Car ())
```

The interface declaratively shows what attributes and methods the object should have. Moreover, the class implements the interface, and the object of the class provides. You should pay attention to the difference between these concepts!

"Implementing" an interface means that only the "produced" entity will have the required properties; and "providing" an interface speaks of the specific capabilities of the entity being evaluated. Accordingly, in Python, classes, by the way, can both implement and provide an interface.

In fact, the implementation declaration (IVehicle) is a convention; just a promise that a given class and its objects behave that way. No real checks will be made.

```python
class IVehicle (zope.interface.Interface):
    """Any moving thing"""
    speed = zope.interface.Attribute("""Movement speed""")

    def move():
        """Make a single step"""

class Car(object):
    zope.interface.implements(IVehicle)

assert IVehicle.implementedBy(Car)
assert IVehicle.providedBy(Car())
```

The component architecture of Zope includes another important concept - adapters. Generally speaking, this is a simple design pattern that corrects one class for use somewhere where a different set of methods and attributes is required.

## Adapters

Consider a simple an example from the [Comprehensive Guide to Zope Component Architecture](https://bluebream.zope.org/doc/1.0/manual/componentarchitecture.html).

Suppose there are a couple of classes, Guest and Desk. Let's define interfaces to them, plus a class that implements the Guest interface:

```python
import zope.interface
from zope.interface import implements
from zope.component import adapts, getGlobalSiteManager

class IDesk(zope.interface.Interface):
    def register():
        "Register a person"

class IGuest(zope.interface.Interface):
    name = zope.interface.Attribute ("""Person`s name""")

class Guest (object):
    implements(IGuest)

    def __init __ (self, name):
        self.name = name
```

The adapter must account for the anonymous guest by registering in the list of names:

```python
class GuestToDeskAdapter (object):
    adapts(IGuest)
    implements(IDesk)
    
    def __init __ (self, guest):
        self.guest = guest
    
    def register (self):
        guest_name_db.append (self.guest.name)
```

There is a registry that keeps track of adapters by interface. Thanks to it, you can get an adapter by passing an adaptable object to the call of the interface class. If the adapter is not registered, the second argument to the interface will be returned:

```bash
guest = Guest ("Ivan")
adapter = IDesk (guest, alternate = None)
print adapter
>>>> None found

gsm = getGlobalSiteManager ()
gsm.registerAdapter (GuestToDeskAdapter)

adapter = IDesk (guest, alternate = "None found")
print adapter

>>>> __ main __. GuestToDeskAdapter object at 0xb7beb64c>
```

This infrastructure is useful for splitting code into components and linking them together.

One of the most striking examples of using this approach besides Zope itself is the Twisted network framework, where a fair amount of the architecture relies on zope.interfaces interfaces.

## Conclusion

Upon closer inspection, it turns out that interfaces and abstract base classes are two different things.

Abstract classes basically hardcode the required front-end part. Checking an object against the interface of an abstract class is checked using the built-in isinstance function; class - issubclass. An abstract base class should be included in the hierarchy as a base class or mixin.

The downside is the semantics of checks issubclass, isinstance, which intersect with ordinary classes (their inheritance hierarchy). No additional abstractions are built on the ABC.

Interfaces are a declarative entity, they do not set any boundaries; simply asserts that the class implements and its object provides the interface. Semantically, the statements implementedBy, providedBy are more correct. On such a simple basis, it is convenient to build a component architecture using adapters and other derived entities, which is what the large Zope and Twisted frameworks do.

It should be understood that the use of both tools makes sense only when building and using relatively large OOP systems - frameworks and libraries, in small programs they can only confuse and complicate the code with unnecessary abstractions.
