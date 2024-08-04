**S** - Single Responsibility Principle
**O** - Open closed principle
**L** - Liskov substitution principle
**I** - Interface Segregation principle
**D** - Dependency Inversion principle

### Single Responsibility Principle
A class should have one and only one reason to change, meaning that a class should have only one job.
```
class Square
{
    public $length;
}
>>>>>>>>>>>>>>>>>>>>>>>>>>.

class Circle
{
    public $length;
}


class AreaCalculator 
{

    public function sum()
    {
        foreach ($this->shapes as $shape) {
            if (is_a($shape, 'Square')) {
                $area[] = pow($shape->length, 2);
            } elseif (is_a($shape, 'Circle')) {
                $area[] = pi() * pow($shape->radius, 2);
            }
        }

        return array_sum($area);
    }
}
```



### Open closed principle
Objects or entities should be open for extension but closed for modification. This means that a class should be extendable without modifying the class itself.

above AreaCalculator  is calculating area for both square and circle. if new shape  need to be added, existing AreaCalculator class needs to be modified. better add sum method in each shape class itself. 

```
class Square
{
    public $length;
    public function area()
    {
        return pow($this->length, 2);
    }
}
>>>>>>>>>>>>>>>>>>>>>>>>>>.

class Circle
{
    public $length;
     public function area()
    {
        return pi() * pow($shape->radius, 2);
    }	
}


class AreaCalculator 
{

    public function sum()
    {
        foreach ($this->shapes as $shape) {
            $area[] = $shape->area();
        }
        return array_sum($area);
    }
}
```

**How do we verify if all shape class send to find AreaCalculator.sum() has area method??**
**then the interface concept comes into picture**

```

interface ShapeInterface
{
    public function area();
}

class Square implements ShapeInterface{
    public $length;
    public function area()
    {
        return pow($this->length, 2);
    }
}


class Circle implements ShapeInterface{
    public $length;
     public function area()
    {
        return pi() * pow($shape->radius, 2);
    }	
}


class AreaCalculator 
{

    public function sum()
    {
        foreach ($this->shapes as $shape) {
            if (is_a($shape, 'ShapeInterface')) {
                $area[] = $shape->area();
                continue;
            }

            throw new AreaCalculatorInvalidShapeException();
        }
        return array_sum($area);
    }
}
```
```
```

 ### Liskov substitution principle
Let q(x) be a property provable about objects of x of type T. Then q(y) should be provable for objects y of type S where S is a subtype of T.

if class A is a subtype of class B, you should be able to replace B with A without breaking the behavior of your program
```
```java
interface Bike {
    void turnOnEngine();

    void accelerate();
}
```


```
```java
class Motorbike implements Bike {

    boolean isEngineOn;
    int speed;

    @Override
    public void turnOnEngine() {
        isEngineOn = true;
    }

    @Override
    public void accelerate() {
        speed += 5;
    }
}
```

```java
class Bicycle implements Bike {

    boolean isEngineOn;
    int speed;

    @Override
    public void turnOnEngine() {
        throw new AssertionError("There is no engine!");
    }

    @Override
    public void accelerate() {
        speed += 5;
    }
}
```

`Bicycle` class throws an `AssertionError` in the `turnOnEngine()` method because it has no engine. This means that an instance of `Bicycle` cannot be substituted for an instance of `Bike` without breaking the behavior of the program.
The `Bicycle` class is considered a subtype of the `Bike` interface, then according to the LSP, any instance of `Bike` should be replaceable with an instance of `Bicycle`
But in this case, it's not true because `Bicycle` throws an `AssertionError` while trying to turn on the engine. Therefore, the code violates the LSP.


### Interface segregation principle
A client should never be forced to implement an interface that it doesn’t use, or clients shouldn’t be forced to depend on methods they do not use.
```
interface ShapeInterface
{
    public function area();

    public function volume();
}
```

If shape interface is implemented by Square. Square need to implement volume though it can not have its volume. 
```
interface ShapeInterface
{
    public function area();
}

interface ThreeDimensionalShapeInterface
{
    public function volume();
}
```

This interface segregation solves this problem
```
class Cuboid implements ShapeInterface, ThreeDimensionalShapeInterface
{
    public function area()
    {
    }

    public function volume()
    {

    }
}
```

```
class Square implements ShapeInterface
{
    public function area()
    {
    }
}
```

### Dependency inversion principle states:

The Dependency Inversion Principle (DIP) states that **high-level modules should not depend on low-level modules, but both should depend on abstractions**. Abstractions should not depend on details – details should depend on abstractions.

For example, consider a scenario where you have a class that needs to use an instance of another class. In the traditional approach, the first class would directly create an instance of the second class, leading to a tight coupling between them. This makes it difficult to change the implementation of the second class or to test the first class independently.

But if you apply the DIP, the first class would depend on an abstraction of the second class instead of the implementation. This would make it possible to easily change the implementation and test the first class independently.

https://www.digitalocean.com/community/conceptual-articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design