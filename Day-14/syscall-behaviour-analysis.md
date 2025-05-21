## Syscall Behaviour Analysis

- Even if we are securing our Cluster using System Hardening, Container Sandboxing, Using mTLS, and Restricting Network access to the Cluster, we still need to ensure that our cluster remains secure. There will always be a chance for attackers to exploit the vulnerabilities in the Cluster. So if any event occurs in the Cluster, we need to be able to detect it and take action. We can use Syscall Behaviour Analysis to detect any malicious activity in the Cluster. Syscall Behaviour Analysis is a technique that allows us to monitor the system calls made by a process and detect any malicious activity. We can use Syscall Behaviour Analysis to detect any malicious activity in the Cluster.
- Detecting the event sooner can help us to mitigate the risk and prevent the attack from spreading. 

### Falco 

- Falco is a tool from Sysdig that provides a way to monitor the system calls made by a process and detect any malicious activity. 
- Example: If a container is trying to access a file that it should not be accessing like `/etc/shadow` or Deleting Audit Calls, Falco will detect it and alert us.
- Falco can send alerts to various channels like Slack, PagerDuty, and others.

Date of Commit: 20/05/2025