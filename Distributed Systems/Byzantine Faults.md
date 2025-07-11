# Explanation
- [[Distributed Systems|Distributed systems]] problems become much harder if there is a risk that nodes may “lie” (send arbitrary faulty or corrupted responses).
- Such behavior is known as a *Byzantine fault*, and the problem of reaching consensus in this untrusting environment is known as the *Byzantine Generals Problem*.
- A system is Byzantine fault-tolerant if it continues to operate correctly even if some of the nodes are malfunctioning and not obeying the protocol, or if malicious attackers are interfering with the network.
- Protocols for making systems Byzantine fault-tolerant are quite complicated.
- In most server-side data systems, the cost of deploying Byzantine fault-tolerant solutions makes them impracticable.
- In peer-to-peer networks, where there is no such central authority, Byzantine fault tolerance is more relevant.

### The Byzantine Generals Problem
- The Byzantine Generals Problem is a generalization of the so-called Two Generals Problem:
	- Two army generals need to agree on a battle plan.
	- As they have set up camp on two different sites, they can only communicate by messenger, and the messengers sometimes get delayed or lost.
- In the Byzantine version of the problem, there are $n$ generals who need to agree, and their endeavor is hampered by the fact that there are some traitors in their midst.
- Most of the generals are loyal, and thus send truthful messages, but the traitors may try to deceive and confuse the others by sending fake or untrue messages (while trying to remain undiscovered).
- It is not known in advance who the traitors are.

# Sources
- Designing Data-Intensive Applications - Chapter 8
