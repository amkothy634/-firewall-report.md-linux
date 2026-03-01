 Firewall Configuration Report
Default policies
Rule: ufw default deny incoming  
What: Blocks all incoming connections unless explicitly allowed.
Why: Protects the server by ensuring only required services are reachable.
Rule: ufw default allow outgoing  
What: Allows all outbound connections.
Why: Lets the server access updates and external services without extra rules.

Allowed services
Rule: ufw allow ssh  
What: Allows incoming SSH connections on port 22.
Why: Needed for remote administration.
Rule: ufw limit ssh  
What: Allows SSH but rate‑limits repeated attempts.
Why: Helps protect against brute‑force login attacks.
Rule: ufw allow 80/tcp  
What: Allows incoming HTTP traffic.
Why: Required for serving web pages.
Rule: ufw allow 443/tcp  
What: Allows incoming HTTPS traffic.
Why: Required for secure encrypted web traffic.

Logging
Rule: ufw logging medium  
What: Enables logging for blocked packets, allowed packets, and rate‑limited events.
Why: Required by the assignment and useful for monitoring suspicious activity.

Advanced protection rules (added to /etc/ufw/before.rules)
Block ICMP echo requests (ping)
-A ufw-before-input -p icmp --icmp-type echo-request -j DROP
What: Drops incoming ping requests.
Why: Prevents ping scans used to discover active hosts.

SYN flood protection
-A ufw-before-input -p tcp --syn -m limit --limit 10/s --limit-burst 20 -j ACCEPT
-A ufw-before-input -p tcp --syn -j DROP
What: Limits the rate of SYN packets and drops excessive SYN traffic.
Why: Protects the server from SYN flood attacks, a common DoS method.

Drop invalid packets
-A ufw-before-input -m conntrack --ctstate INVALID -j DROP
What: Drops malformed or invalid packets.
Why: These packets are often used in scanning or malformed‑packet attacks.

Verification
Command: ufw status verbose  
What: Shows active rules, logging level, and allowed services.
Why: Confirms the firewall is enabled and configured correctly.

Command: less /var/log/ufw.log  
What: Displays firewall log entries.
Why: Verifies that blocked and allowed traffic is being logged.

Conclusion
The firewall is configured to load at startup, allow required services (SSH, HTTP, HTTPS),
log all relevant traffic, and protect against SYN floods, invalid packets, and brute‑force attacks. 
This setup meets all assignment requirements and improves the server’s security.
