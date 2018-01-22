## Result Analysis on load testing using Taurus with Jmeter

In this initial phase of performance testing assignment, I have done a bit of research to choose Taurus as performance testing tool of my choice since, Taurus has a good test result reporting capabilities, easy to Integrate into continuous integration pipeline, simplicity, and can run with existed functional
or non functional testing Frameworks like Selenium and Gatling.

Taurus test can easily be set up by converting JMeter performance test plan in the YAML file script.
The actual test can be executed in the command line or NON-GUI mode.For detail steps to set up and readings refer https://github.com/jemalft/-Users-jemal-jemal-performance-project-performance-projects

In this sample load testing exercise, I will try to analyze test results and identify the bottlenecks where concurrent users would be failed to upload 10mb of CSV file where an abrupt increase in average response time and latency.
###
Environment specifications:

```
MacBook Pro (Retina, 15-inch, Mid 2015)
Processor : 2.8 GHz Intel Core i7
Number of Processors = 1 
Number of core  = 4
RAM Memory: 16 GB
```

Table 1: File upload scenario using REST API to simulate Jmeter virtual users(VU), in 5 VU increments to maximum of 25VU

|Scenario #| Concurrency(VU)	| Ramp up time(s)
---------- |---------------   |----------------
|1	       | 5 	              | 25
|2	       | 10 	            | 50
|3	       | 15 	            | 75
|4	       | 20 	            | 100
|5	       | 25	              | 125

Figure 1 : Active Users vs Average response time 

![alt png](https://github.com/jemalft/-Users-jemal-jemal-performance-project-performance-projects/blob/master/Average-response-time.png)

As shown in figure 1, The average response time is increasing when concurrent users go up from 5 to 15.However, when an additional 5 more active users sending 10mb of file uploading request through REST API, the server start behaving differently and the average response time growing exponentially. Despite, that the server keeps responding the incoming HTTP requests in a very slow fashion until the number of concurrent users has grown beyond 20.

When JMeter injected last 5 users(scenario#5) that literally boosted and challenging the application load performance. Due to this, the network traffic getting higher and higher by the incoming requests to hit the server. Eventually, the server starts complaining and threw 500 internal server error and “socketException closed” respectively. All this server response has proved the app bottleneck point when uploading a file by the number of concurrent users just beyond 20.

Figure 2 :  Active Users vs Average Latency, Aggregate behavior when user growing from 5 to 25

![alt png](https://github.com/jemalft/-Users-jemal-jemal-performance-project-performance-projects/blob/master/Simulation-latency.png)


As shown in Figure 2, Average latency goes to zero when the number of concurrent users reached to 25 because the application accepted high network traffic that hitting the server.On the other hand, the graph latency is grown when user rises from 5 to 20 concurrent users, this makes sense alike average response time is decreasing.

## Summary

In this simple experiment, we are able to identify the application critical point where the network performance quite low and eventually closed due to incoming requests traffic and requests under queue in the server gateway. Based on the result, This can lead as to a conclusion we are facing in a nonnegligible amount of network performance issues for this specific scenario.

Why this happened, due to the number of active concurrent user's request hitting the server reached to the maximum that our application could handle.The remark to a possible solution is clear, to improve the network performance for the specified number of concurrent users. 
