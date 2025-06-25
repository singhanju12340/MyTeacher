
Create a Spring Boot application with the EnableAgent annotation: This annotation transforms your standard Spring application into an agentic application, enabling it to participate in A2A and MCP-based workflows without changing your existing service logic.

```Java
package io.github.vishalmysore;  
import io.github.vishalmysore.tools4ai.EnableAgent;  
import org.springframework.boot.SpringApplication;  
import org.springframework.boot.autoconfigure.SpringBootApplication;  
@SpringBootApplication  
@EnableAgent  
public class Application {  
}
```



Java MCP client 
![[Screenshot 2025-06-13 at 1.57.46 PM.png]]