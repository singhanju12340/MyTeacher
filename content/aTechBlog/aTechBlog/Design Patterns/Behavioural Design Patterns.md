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
