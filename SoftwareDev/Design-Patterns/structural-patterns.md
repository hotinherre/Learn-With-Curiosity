# Structural patterns

manage relationships between entities.

## Adaptor

Convert an interface of a class into another interface client expected. Build an intermediary abstraction that map old component into new system.

## Bridge

### Description

Decouple abstraction from its implementation so that both can vary independently.

### Example:

![](https://sourcemaking.com/files/v2/content/patterns/Bridge_.png)

```Java
Sword enchantedSword = new Sword(new SoulEatingEnchantment());
Hammer hammer = new Hammer(new FlyingEnchantment());
```

## Composite

### Description

Compose objects into tree structures to represent whole hierarchy. Composite lets clients treat individual objects and compositions of objects uniformly.

### Example:

Every sentence is composed of words which are in turn composed of characters. Each of these objects is printable and they can have something printed before or after them like sentence always ends with full stop and word always has space before it

## Decorator

### Description

Add additional responsibility into object dynamically.  
Decorator pattern lets you dynamically change the behavior of an object at run time by wrapping them in an object of a decorator class.

### Example:

here is an angry troll living in the nearby hills. Usually it goes bare handed but sometimes it has a weapon. To arm the troll it's not necessary to create a new troll but to decorate it dynamically with a suitable weapon.

```java
Troll troll = new SimpleTroll();
// change the behavior of the simple troll by adding a decorator
Troll clubbedTroll = new ClubbedTroll(troll);
```
