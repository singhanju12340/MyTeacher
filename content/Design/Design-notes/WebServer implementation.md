---
Creation Time: Thursday, December 5th 2024
Modified Time: Thursday, December 5th 2024
---
#### Phesudo code
s = listen(8088);  //  waiting for TCP connection on 8088 

for( ){
		conn = accept();  // waiting for TCP connection 
		req = c.read(); '// read the request from socket 
		response = execute(req);
		conn.write(response)

}

`This is single-threaded HTTP server. It is handling 1 connection at a time.

for( ){
		conn = accept();  // waiting for TCP connection 
			thread  t2 = new thread();
			t2.start();

		// connection object is free now
		

}

runnable(){
	req = c.read(); '// read the request, most expensive and time taking operations
	response = execute(req);
	return response;

}

In single-threaded example, we moved some execution apart from socket connection work we moved it to another thread and freed socket to connect to other requests. How do we implement it without having a separate thread for an execution? 
How does javascript single threaded implementation work.

Javascript uses Epoll. If there is any file descriptor ready with data then only read starts and it helps in I/O multiplexing.







