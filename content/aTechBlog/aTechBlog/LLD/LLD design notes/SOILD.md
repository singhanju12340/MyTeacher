S - Single Responsibility Principle
O - Open closed principle
L - Liskov substitution principle
I - Interface segregation principle
D - Dependency Inversion principle

**Single Responsibility Principle**
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



**Open closed principle**
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

 **Liskov substitution principle**
Let q(x) be a property provable about objects of x of type T. Then q(y) should be provable for objects y of type S where S is a subtype of T.

**Interface segregation principle**
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

**Dependency inversion principle states:**

Entities must depend on abstractions, not on concretions. It states that the high-level module must not depend on the low-level module, but they should depend on abstractions.

https://www.digitalocean.com/community/conceptual-articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design