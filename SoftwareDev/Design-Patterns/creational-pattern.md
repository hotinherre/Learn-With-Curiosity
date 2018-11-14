# Creational Design Patterns

All about class instantiation

## Abstract Factory

### Description

Provide an interface to create families of related objects without specifying their concreate classes.  
Use a factoryMaker method to return instance of specific factory.

### Example:

Use KindomFactory to create differct object of a kindom: king, army and castle.

```java
KingdomFactory factory = FactoryMaker.makeFactory(KingdomType.ELF));
Castle castle = factory.createCastle();
King king = factory.createKing();
Army army = factory.createArmy();
```

## Factory Method

### Description

Define a method to create single object without specifying their concrete classes.

### Example:

Blacksmith use same equipment to creates different weapon.

```java
Blacksmith blacksmith = new ElfBlacksmith();
blacksmith.manufactureWeapon(WeaponType.SPEAR);
blacksmith.manufactureWeapon(WeaponType.AXE);
```

## Builder

### Description

Allows you to create a different flavor object without construtor pollution.  
Decouple the creation of an object and its representation.

### Example:

We need to create a game charactor with different attribute.

```java
public Hero(Profession profession, String name, HairType hairType,
            HairColor hairColor, Armor armor, Weapon weapon) {}
```

Here is a long list of constuctor parameters and the list can keep growing when more options is Added.  
Hard to manage the arrangement of the parameters.

Here Builder comes to the scene.

```java
Hero mage = new Hero.Builder(Profession.MAGE, "Riobard")
                .withHairColor(HairColor.BLACK)
                .withWeapon(Weapon.DAGGER)
                .build();
```

## Object Pool

### Description

When object instantiation is high and object is only needed for a short period of time. Using object pool to manage object caching. Object Pool is singtle class.

#### Method

- acquireReusable: Client request object from object pool. Object Pool instantiate new object if no object available in cache.
- releaseReusable: Client release object back to object Pool.
- setMaxPoolSize: limit total number of reusable objects.

### Example:

Assign desk cells to new hire, collect desk cell when employee leaves.

## Prototype

### Description

Create a copy of an existing object and modify it to your needs, instead of going through the trouble of creating new object from scratch.

### Example:

```python
dispatcher = PrototypeDispatcher()
prototype = Prototype()
d = prototype.clone()
a = prototype.clone(value='a-value', category='a')
b = prototype.clone(value='b-value', is_checked=True)
```

## Singleton

### Description

ensure a class only has one instance and provide a global point of access to it.

### Example:

There should be only one reusable db connection in the entire system.
