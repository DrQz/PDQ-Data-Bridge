# Tomcat JMX


#### Author:   Mohit Chawla (Graduated GCAP 2016, GDAT and PDQW 2017)
#### Created:  Saturday, October 06, 2018 
#### Status:   Work in progress 


### Background
Tomcat's default JMX interface has a lot of useful metrics oganized in MBeans. Its helpful to accustom yourself with their structure, by using tools such as <code>jmxcli</code> and <code>Java Management Console</code>. It is also possible to extend and customise MBeans - this can be a big advantage when doing in-depth performance analysis.

### Data 
The <code>GlobalRequestProcessor</code> MBean (for each connector, for eg., <code>ajp-bio-8009</code>) consists of the most important metrics to be used by PDQ - however, note that this MBean contains aggregate numbers across all the application endpoints - so if you have multiple endpoints with different workloads and behavior, you will have to collect the data yourself across the various RequestProcessor MBeans.  

1. requestCount

This is the total number of requests - convert this to a rate metric to get the measured throughput, or <code>Xdat</code>.

2. processingTime

This is total processing time for ALL requests - divide this by requestCount for a common time interval - to get the average request processing time, or <code>Rdat</code>.

3. Service Threads or Concurrency

This is an advanced metric that gives the concurrency of the application - it's not directly available in the standard MBeans interface - however, you can write a script. One way is to fetch the data from the JMX interface over HTTP and track store its state over time. You could use the <code>/manager/status</code> endpoint to see all the data. The total number of all the request threads in "Service" stage is the concurrency of the application. Alternatively - you could also get this data from Threading related MBeans - all threads in state number "3" are service threads - the number mapping is not documented, but you can check the source code for the same. For confirming the validity of your data, use Little's Law (<code>Xdat * Rdat</code>) - these quantities should be almost same.

4. CPU Utilization and Service Time

This is the CPU utilization <code>Udat</code> as reported by Linux. Use Little's Law as <code>Sest = Udat/Xdat</code> to estimate the service time.



