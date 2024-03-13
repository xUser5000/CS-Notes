# Explanation

## Styles of Connection Release
- There are two styles of terminating a connection:
	- **Asymmetric Release**: when on party hangs up, the connection is broken.
		- May result in data loss if a host sends segments before receiving a **DISCONNECTION REQUEST**.
		- The telephone system works like this.
	- **Symmetric Release** treats the connection as two separate unidirectional connections and requires each one to be released separately.
		- A host can continue to receive data even after it has send a **DISCONNECT** segment.
		- Works only when each process has a fixed amount of data to send and clearly knows when it has sent it.

## The Two-Army Problem

### Illustration
![[The two-army problem.png]]

### Description
- Imagine that a white army is encamped in a valley.
- On both of the surrounding hillsides are blue armies.
- The white army is larger than either of the blue armies alone, but together the blue armies are larger than the white army.
- If either blue army attacks by itself, it will be defeated, but if the two blue armies attack simultaneously, they will be victorious.
- The blue armies want to synchronize their attacks but the only communication medium is to send messengers on foot down into the valley, where they might be captured and the message is lost (i.e., they have to use an unreliable communication channel).
- **The question is: does a protocol exist that allows the blue armies to win?**

- Suppose that the commander of blue army #1 sends a message reading: ‘‘I propose we attack at dawn on March 29. How about it?’’
- Now suppose that the message arrives, the commander of blue army #2 agrees, and his reply gets safely back to blue army #1.
- Will the attack happen? Probably not, because commander #2 does not know if his reply got through.
	- If it did not, blue army #1 will not attack, so it would be foolish for him to charge into battle.

- Now let us improve the protocol by making it a three-way handshake where the initiator of the original proposal must acknowledge the response.
- Assuming no messages are lost, blue army #2 will get the acknowledgement, but the commander of blue army #1 will now hesitate.
	- After all, he does not know if his acknowledgement got through, and if it did not, he knows that blue army #2 will not attack.

- We could now make a four-way handshake protocol, but that does not help either.

### Solution
- **It can be proven that no protocol exists that solves the problem**.

#### Proof
- Suppose that some protocol did exist.
- Either the last message of the protocol is essential, or it is not.
- If it is not, we can remove it (and any other unessential messages) until we are left with a protocol in which every message is essential.
- What happens if the final message does not get through?
	- We just said that it was essential, so if it is lost, the attack does not take place.
- Since the sender of the final message can never be sure of its arrival, he will not risk attacking.
	- Worse yet, the other blue army knows this, so it will not attack either.

### Practical Implications
- If neither side is prepared to disconnect until it is convinced that the other side is prepared to disconnect too, the disconnection will never happen.
- In practice, we can avoid this quandary by foregoing the need for agreement and pushing the problem up to the [[Transport Layer|transport]] user, letting each side independently decide when it is done.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 6.2.3
