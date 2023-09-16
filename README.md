# Building-Secure-Reliable-Systems
Designing, Implementing  and Maintaining Systems

#CHAPTER 1
#The Intersection of Security and Reliability

On Password and Drills

On September 27, 2012, an innocent Google-wide announcement caused a series of cascading failures in an internal service. Ultimately, recovering from these failures required a power drill.

Google has an internal password manager that allows employees to store and share secrets for third-party services that don't support better authentication mechanisms. One Such secret is the
password to the guest WiFi system on the large fleet of buses that connect Google's San Francisco Bay Area campuses.

On that day in September, the corporate transportation team emailed an announcement to thousands of employees that the WiFi password had cahnged. The resulting spike in traffic ws far larger
than the password management system--which had been developed years earlier for a small audience of system administrators--could handle.

The load caused the primary replica of the password manager to become unresponsive, so the load balancer diverted traffic to the secondary replica, which promptly failed in the same way. 
At this point, the system paged the on-call engineer. The engineer had no experience responding to failures of the service: the password manager was supported on a best-effort basis, and
had never suffered an outage in its five years of existence. The engineer attempted to restart the service, but did not know that a restart required a hardware security module (HSM) smart card.
These smart cards were stored in multiple safes in different Google offices across the globe, but not in New York City, where the on-call engineer was located. When the service failed to restart,
the engineer in Australia could not open the safe because the combination was stored in the now-offline password manager. Fortunately , another colleague in  California had memorized the 
combination to the on-site safe and was able to retrieve a smart card. However, even after the engineer in caifornia inserted the card into a reader, the service still failed to restart
with the cryptic error, "The password could not load any of the cards protecting this key."

At this point, thr engineer in australia decided that a brute-force approach to their safe problem was warranted and applied a power drill to the task. An hour later, the safe was open--but
even the newly retrived cards triggered the same error message.

It took an additional hour for the team to realize that the green light on the smart card reader did not, in fact, indicate that the card had been inserted correctly. When the engineer 
flipped the card over, the service restarted and the outage ended.

Reliability and Security are both crucial components of a truly trustworthy system, but building systems that  are both reliable and secure is difficult. While the requirements for reliability
and security share many common properties, they also require different design considderations. It is easy to miss the subtle interplay manager's failure was triggered by a reliability problem--poor
load-balancing and load-shedding stratigies--and its recovery was later complicated by multiple measures designed to increase the security of the system.
