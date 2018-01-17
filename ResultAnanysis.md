# Result Analysis sample load testing using Taurus with Jmeter

As my initial phase of performance testing practices, I have done a bit of research to choose Taurus as performance testing tool of my choice since, 
Taurus has a good test result reporting capabilities, easy to Integrate into continuous integration pipeline, simplicity, and can run with existed functional
or non functional testing Frameworks like Selenium and Gatling.

Taurus test can easily be set up by converting Jmeter performance test plan in the YAML file script.
The actual test will be executed from command window in NON-GUI mode.For detail steps and reading https://github.com/jemalft/-Users-jemal-jemal-performance-project-performance-projects

In this sample load testing exercise, I will try to analyzing test results and identify the bottlenecks where concurrent users would be failed to upload CSV file and abruptly rise in response time and latency.
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

Figure 2 :  Active Users vs Average Latency, Aggregate behavior when user growing from 5 to 25

![alt png](https://github.com/jemalft/-Users-jemal-jemal-performance-project-performance-projects/blob/master/Simulation-latency.png)
