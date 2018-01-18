## Result Analysis on sample load testing using Taurus with Jmeter

As my initial phase of performance testing practices, I have done a bit of research to choose Taurus as performance testing tool of my choice since, 
Taurus has a good test result reporting capabilities, easy to Integrate into continuous integration pipeline, simplicity, and can run with existed functional
or non functional testing Frameworks like Selenium and Gatling.

Taurus test can easily be set up by converting Jmeter performance test plan in the YAML file script.
The actual test will be executed from command window in NON-GUI mode.For detail steps and reading https://github.com/jemalft/-Users-jemal-jemal-performance-project-performance-projects

In this sample load testing exercise, I will try to analyzing test results and identify the bottlenecks where concurrent users would be failed to upload CSV file and abruptly rises its average response time and latency.
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

As shown on figure 1, The avarage response time is increasing in fair fashion when concurrent users load rise from 5 to 15.However after the load grown beyond 15 users,the server start behaving differently and the response time start growing exponsetially for the next 5 user increment.execept a higher average response time and latency the server keep working slowly until that point.

When Jmeter injected additional 5 users(scenario#5) to challenge the application performance,the network traffic became very high and the system get crashed and the server start to complaining by throwing 500 internal server error and “socketException closed” respectively.Perhaps the application botleneck point aligned on when uploading a file for number of concurrent users reached 21.

Figure 2 :  Active Users vs Average Latency, Aggregate behavior when user growing from 5 to 25

![alt png](https://github.com/jemalft/-Users-jemal-jemal-performance-project-performance-projects/blob/master/Simulation-latency.png)


As shown on Figure 2, Average latency goes to zero because the app looks crashed and not accepting and responding to any request.Look how the graph latency is grown when user rise from 5 to 20 concurrent users, this makes sense alike average response time is decreasing.

## Summary

In this simple experiment,we are able to identify the application critical point where the network performance quit low and eventually closed the incoming traffic and requests under que of REST API calls.Based on the result,This can lead as to a conclusion we are facing a performance issue dues to low newtwork performance.

Why this happened , due to the number of active virtual users or concurrent HTTP request calls seem reached at maximum that our app can handles.and the suggested solution is very clear we have to improve the network performance to eradicate the issue. 

