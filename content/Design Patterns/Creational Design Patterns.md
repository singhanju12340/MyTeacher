1. ![[Screenshot 2024-07-02 at 6.04.26 PM.png]]
2. 
3. 
4. ![[Screenshot 2024-07-02 at 6.11.55 PM.png]]

Sure! Here's an example of the Factory Pattern in Java. The Factory Pattern is a creational design pattern that provides an interface for creating objects, but allows subclasses to alter the type of objects that will be created.

### Scenario:

Let's create a system for a logistics company where different types of vehicles (e.g., Truck, Ship) can be used for transportation. We'll use the Factory Pattern to create these vehicle objects.

### Components:

1. **Product Interface**: Declares the interface for objects that the factory method creates.
2. **Concrete Product Classes**: Implement the `Product` interface.
3. **Creator (Factory) Interface**: Declares the factory method.
4. **Concrete Creator (Factory) Classes**: Implement the factory method to create specific products.


### Explanation:

- **Product Interface**: The `Transport` interface declares the `deliver` method.
- **Concrete Product Classes**: The `Truck` and `Ship` classes implement the `Transport` interface and provide specific implementations of the `deliver` method.
- **Creator (Factory) Interface**: The `Logistics` abstract class declares the factory method `createTransport` and a method `planDelivery` that uses the product created by the factory method.
- **Concrete Creator (Factory) Classes**: The `RoadLogistics` and `SeaLogistics` classes implement the factory method `createTransport` to create specific products (`Truck` and `Ship` respectively).
- **Client Class**: The `Client` class uses the factory classes to create and use products.

### Advantages of Factory Pattern:

1. **Encapsulation**: The Factory Pattern encapsulates the object creation process, hiding it from the client.
2. **Decoupling**: It decouples the client code from the object creation code, making the system more flexible and easier to extend.
3. **Single Responsibility Principle**: It adheres to the Single Responsibility Principle by delegating the responsibility of object creation to specific factory classes.
4. **Open/Closed Principle**: The system can be extended with new product types without modifying existing code, adhering to the Open/Closed Principle.