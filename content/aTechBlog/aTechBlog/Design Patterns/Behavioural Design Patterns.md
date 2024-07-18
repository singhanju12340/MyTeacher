1. Factory Design Patterns
#### Command Interface

```
public interface Command { 
	void execute(); 
}

public class LightOnCommand implements Command 
{
	private Light light; 
	public LightOnCommand(Light light) { 
			this.light = light;
		 } 
		 @Override public void execute() { 
		 light.on(); 
		 }
 }

public class LightOffCommand implements Command {
	private Light light;
	public LightOffCommand(Light light) 
	{ 
		this.light = light; 
	} 
	@Override public void execute() {
		light.off(); 
	}
}



```


```#### Receiver Classes

java

Copy code

`// Light Receiver 
public class Light {  

	public void on() {        
		System.out.println("The light is on");     
	}     
	public void off() {       
		System.out.println("The light is off");     
	} 
}
`



#### 4. Invoker Class


`public class RemoteControl {
	private Command command;     
	 public void setCommand(Command command) {  
	        this.command = command;    
	}      
	         
	public void pressButton() { 
	        command.execute();   
	  } 
}`


Client code

// Receivers
Light livingRoomLight = new Light();
Fan ceilingFan = new Fan(); 

// Commands
Command lightOn = new LightOnCommand(livingRoomLight); 
Command lightOff = new LightOffCommand(livingRoomLight);

RemoteControl remote = new RemoteControl();
// Test Light On
remote.setCommand(lightOn);
remote.pressButton();

```


By using the Command Pattern, we can easily extend the system to support new appliances or new types of commands without modifying the existing code. This makes the system flexible and maintainable.


## Strategy Patterns:

It allows you to dynamically change the behavior of an object by encapsulating it into different strategies

It is based on the principle of composition over inheritance. It defines a family of algorithms, encapsulates each one, and makes them interchangeable at runtime.The core idea behind this pattern is to separate the algorithms from the main object. This allows the object to delegate the algorithm's behavior to one of its contained strategies.

### Use Cases for the Strategy Design Pattern

The Strategy Design Pattern can be useful in various scenarios, such as:

1. **Sorting algorithms**: Different sorting algorithms can be encapsulated into separate strategies and passed to an object that needs sorting.
2. **Validation rules**: Different validation rules can be encapsulated into separate strategies and passed to an object that needs validation.
3. **Text formatting**: Different formatting strategies can be encapsulated into separate strategies and passed to an object that needs formatting.
4. **Database access**: Different database access strategies can be encapsulated into separate strategies and passed to an object that needs to access data from different sources.
5. **Payment strategy**: Different payment methods can be encapsulated into separate strategies and passed to an object that needs to process payments.

To implement the Strategy Design Pattern in Java, follow these steps:

1. Identify the algorithm or behavior that needs to be encapsulated and made interchangeable.
2. Define an interface that represents the behavior, with a single method signature that takes in any required parameters.
3. Implement concrete classes that provide specific implementations of the behavior defined in the interface.
4. Define a context class that holds a reference to the interface and calls its method when needed.
5. Modify the context class to allow for the dynamic swapping of the concrete implementations at runtime.
``` 
java package withoutstrategy;

public class PaymentProcessor {
    private PaymentType paymentType;

    public void processPayment(double amount) {
        if (paymentType == PaymentType.CREDIT_CARD) {
            System.out.println("Processing credit card payment of amount " + amount);
        } else if (paymentType == PaymentType.DEBIT_CARD) {
            System.out.println("Processing debit card payment of amount " + amount);
        } else if (paymentType == PaymentType.PAYPAL) {
            System.out.println("Processing PayPal payment of amount " + amount);
        } else {
            throw new IllegalArgumentException("Invalid payment type");
        }
    }

    public void setPaymentType(PaymentType paymentType) {
        this.paymentType = paymentType;
    }
}

enum PaymentType {
    CREDIT_CARD,
    DEBIT_CARD,
    PAYPAL
}
```

The problem with this code is that it violates the [Open-Closed Principle](https://www.freecodecamp.org/news/open-closed-principle-solid-architecture-concept-explained/), which states that classes should be open for extension but closed for modification. In this code, if you want to add a new payment type, you would have to modify the `processPayment` method, which violates the Open-Closed Principle.

The `PaymentProcessor` class violates the Strategy pattern by using conditional statements to determine the type of payment and then processing it accordingly.

``````java
package withstrategy;

public interface PaymentStrategy {
    void processPayment(double amount);
}


public class CreditCardPaymentStrategy implements PaymentStrategy {
    public void processPayment(double amount) {
        System.out.println("Processing credit card payment of amount " + amount);
    }
}


public class DebitCardPaymentStrategy implements PaymentStrategy {
    public void processPayment(double amount) {
        System.out.println("Processing debit card payment of amount " + amount);
    }
}


public class PaypalPaymentStrategy implements PaymentStrategy {
    public void processPayment(double amount) {
        System.out.println("Processing PayPal payment of amount " + amount);
    }
}

public class PaymentProcessor {
    private PaymentStrategy paymentStrategy;

    public PaymentProcessor(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void processPayment(double amount) {
        paymentStrategy.processPayment(amount);
    }
}
```

