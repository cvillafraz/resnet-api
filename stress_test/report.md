# Stress testing report
Welcome. Let's walk through how I ran stress tests on this API using locust, and what results I got. First of all, what are the specifications of the machine I ran the tests on?

## Machine specifications
- Processor: Intel(R) Core(TM) i5-1035G1 
- Storage: 512 GB SSD  
- RAM: 8 GB
- OS: Linux Mint 20.3 mate

With those specs in mind, let's examine what tests were run, and what were the results.

## Tests defition and results
The locustfile.py in this folder contains 3 tests. One makes a get request to the index route. The other tests make a post request to the index and predict route.

Now, what was the result of the test runs? Did scaling the ML service have an impact?

## Results before scaling
Running the tests with 30 users and a spawn rate of 3 produced the following response rate chart. As shown, the median response rate oscilates between 0 and 900 ms.
![](https://i.imgur.com/8b0Oapy.png)

Then, a test run with 300 users and a spawn rate of 30 resulted in this.
![](https://i.imgur.com/VZULISh.png)
As you can see, the median and the 95th percentile of the response rate increase linearly, until they stabilize at somewhere between 20000ms and 30000ms (and it seems to start increasing at the end).

Let's see now if scaling the ML service made a difference
## Results after scaling
After scaling the ML service to 3 instances, I ran the tests with the same number of users. This was the result with 30 users.
![](https://i.imgur.com/OscZDWs.png)
Aside from a peak at the beginning, the median response rate remained stable below 300ms. In this case, the scaling definetely helped. It produced a lower response rate with less variance.

However, did it have an effect when testing with 300 users?
![](https://imgur.com/Ka0CUS1.png)
It turns out the effect is a bit less noticeable. Again, the response rate increases linearly. The difference is that it flattens around 15000ms and 20000ms, which of course is less than without scaling.

## Conclusion
Scaling the ML service obviously helps reduce. However, more testing is required to determine what amount of instances is ideal.
