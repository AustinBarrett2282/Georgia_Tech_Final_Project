# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior

### Network Topology

The following machines were identified on the network:
- Kali
  - **Operating System**: Kali Linux
  - **Purpose**: Conduct Pentest Engagement
  - **IP Address**: 192.168.1.90
- ELK
  - **Operating System**: Ubuntu Linux
  - **Purpose**: Host for Elasticsearch and Kibana
  - **IP Address**: 192.168.1.100
- Target 1
  - **Operating System**: Debian GNU/Linux 8
  - **Purpose**: Host for WordPress
  - **IP Address**: 192.168.1.110
- Capstone
  - **Operating System**: Ubuntu Linux
  - **Purpose**: Vulnerable Web Server
  - **IP Address**: 192.168.1.105


### Description of Targets

The target of this attack was: `Target 1` (192.168.1.110).

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers.

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### Excessive HTTP Errors

![](2021-05-11-15-44-15.png)

Alert 1 is implemented as follows:
  - **Metric**: WHEN count() GROUPED OVER top 5 'http.response.status_code'
  - **Threshold**: IS ABOVE 400
  - **Vulnerability Mitigated**: Brute Force or Enumeration
  - **Reliability**: This is a very reliable alert. Since codes 400 or above are errors from the client or server side, this alert will block out any normal or successful responses.

#### HTTP Request Size Monitor

![](2021-05-11-15-48-55.png)

Alert 2 is implemented as follows:
  - **Metric**: WHEN sum() of http.request.bytes OVER all documents
  - **Threshold**: IS ABOVE 300
  - **Vulnerability Mitigated**: DDOS or Code injection such as XSS via HTTP requests
  - **Reliability**: This alert could create false positives as large HTTP request that are not malicious and legit HTTP traffic could trigger the alarm. Intermediate reliability

#### CPU Usage Monitor

![](2021-05-11-15-52-29.png)

Alert 3 is implemented as follows:
  - **Metric**: WHEN max() OF system.process.cpu.total.pct OVER all documents
  - **Threshold**: IS ABOVE 0.5
  - **Vulnerability Mitigated**: Any process or program, malicious or not, that is taking up too many resources
  - **Reliability**: This is also a very reliable alert. When you can see how much headroom your CPU has you can have a better understanding of the overall health of your system.

