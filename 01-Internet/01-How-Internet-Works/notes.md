# How the Internet Works — Revision Notes

---

## Key Concepts

| Term | Simple Definition |
|------|-------------------|
| Internet | A global network of computers connected to each other |
| IP Address | Unique address for every device on the network (like a home address) |
| Packet | Small chunk of data sent over the network |
| Router | Device that directs packets to the right destination |
| ISP | Internet Service Provider — company that connects you to the internet |
| Protocol | Agreed rules for how computers communicate |
| TCP/IP | Core protocol stack of the internet |
| TCP | Transmission Control Protocol — ensures reliable, ordered delivery |
| IP | Internet Protocol — handles addressing and routing |
| UDP | Like TCP but faster, no guaranteed delivery (used for video/gaming) |

---

## How It Works — Step by Step

1. You type a URL in browser
2. Browser asks DNS for the IP address of that URL
3. DNS returns the IP (e.g., 142.250.80.46 for google.com)
4. Browser opens a TCP connection to that IP (via 3-way handshake)
5. Browser sends an HTTP request over that connection
6. Server processes request and sends back HTTP response
7. Browser renders the response (HTML/CSS/JS)
8. Connection is closed (or kept alive for reuse)

### TCP 3-Way Handshake
- Client → Server: SYN (I want to connect)
- Server → Client: SYN-ACK (OK, acknowledged)
- Client → Server: ACK (Got it, connected)

### Data as Packets
- Data is broken into small packets
- Each packet travels independently across the network
- Packets may take different routes, reassembled at destination
- TCP guarantees all packets arrive and are in order

---

## Industry Examples

- **Netflix** — streams video over the internet using optimized TCP/UDP; UDP for live content (lower latency)
- **WhatsApp** — messages broken into packets, sent over internet, reassembled on receiver's device
- **CDNs (Cloudflare, Akamai)** — sit between users and servers, cache content closer to users to reduce packet travel distance

---

## Interview Questions

**Q1: Can you explain how the internet works at a high level?**
> A: The internet is a global network where devices communicate using TCP/IP. When you request a webpage, your browser finds the server's IP via DNS, opens a TCP connection, sends an HTTP request, and the server returns a response. Data travels in packets across routers.

**Q2: What is the difference between TCP and UDP?**
> A: TCP is reliable — it guarantees delivery and ordering, but has overhead. UDP is faster with no delivery guarantee — used where speed matters more than perfection (video streaming, gaming, DNS queries).

**Q3: What happens if a packet is lost?**
> A: In TCP, the sender is notified (via missing acknowledgment) and retransmits the lost packet. In UDP, there is no retransmission — the application must handle it or accept the loss.

**Q4: What is an IP address and why do we need it?**
> A: An IP address uniquely identifies a device on a network, like a postal address. Without it, routers wouldn't know where to send packets.

**Q5: What is the role of a router?**
> A: A router reads the destination IP on each packet and forwards it to the next router closer to the destination — like a postal sorting office.
