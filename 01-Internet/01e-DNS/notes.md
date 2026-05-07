# 01e — DNS and How It Works

## Key Concepts

| Term | Definition |
|------|-----------|
| DNS | System that translates domain names to IP addresses |
| Recursive Resolver | Server that does the full lookup on your behalf (ISP / 8.8.8.8) |
| Root Name Server | Top of the hierarchy — knows where each TLD server is |
| TLD Server | Knows which authoritative server handles a domain (e.g. all `.com` domains) |
| Authoritative Server | Has the final answer — the actual IP for a domain |
| TTL (Time To Live) | How long a resolver may cache a DNS record before re-asking |
| A Record | Maps domain → IPv4 address |
| AAAA Record | Maps domain → IPv6 address |
| CNAME | Alias — points to another domain name (not an IP) |
| MX Record | Specifies mail servers for a domain |
| NS Record | Lists the authoritative name servers for a domain |
| TXT Record | Arbitrary text — used for domain verification, SPF, DKIM |

---

## How It Works

**Full DNS resolution chain (cache-miss scenario):**

1. Browser checks its own DNS cache
2. OS checks its local DNS cache
3. Request goes to **Recursive Resolver** (ISP or public e.g. 8.8.8.8)
4. Resolver asks **Root Name Server** → returns address of TLD server
5. Resolver asks **TLD Server** (e.g. `.com`) → returns address of authoritative server
6. Resolver asks **Authoritative Server** → returns the actual IP
7. Resolver caches the result (per TTL) and returns IP to browser
8. Browser connects to the IP

> The browser only talks to the recursive resolver. The resolver does all the legwork.

**TTL behaviour:**
- High TTL (86400s) → cached for 1 day, low DNS traffic
- Low TTL (60s) → expires fast, changes propagate quickly, higher DNS load
- Lowering TTL takes effect only after the current TTL expires

---

## DNS Record Types

| Record | Maps | Common Use |
|--------|------|-----------|
| A | domain → IPv4 | Most web servers |
| AAAA | domain → IPv6 | Modern dual-stack servers |
| CNAME | domain → domain | `www.site.com → site.com` |
| MX | domain → mail server | Email routing |
| TXT | domain → text | SPF, DKIM, domain ownership |
| NS | domain → name server | Delegating a zone |

**CNAME vs A Record:**
- A record: points directly to an IP
- CNAME: points to another domain name which resolves to an IP
- If IP changes, update one A record — all CNAMEs follow automatically

---

## Industry Examples

| Company | DNS Usage |
|---------|----------|
| **Netflix** | DNS-based geo-routing — returns different IPs based on user location, directing users to nearest CDN node |
| **AWS Route 53** | Latency-based and geolocation routing; health-check failover via DNS |
| **Cloudflare (1.1.1.1)** | Fast public recursive resolver; also authoritative DNS for millions of domains |
| **Google (8.8.8.8)** | Public recursive resolver, bypasses slow ISP DNS |

---

## Zero-Downtime Migration Pattern

**Scenario:** Moving `api.yourapp.com` to a new server IP

1. Check current TTL on A record (e.g. 86400 = 1 day)
2. **Lower TTL to 60s** — wait the full current TTL duration (24 hrs) for all caches to expire
3. Verify new server is live and healthy
4. **Update A record** to new IP
5. Within 60s, all clients re-resolve and hit new server
6. **Raise TTL back** to 86400 once migration is stable

---

## Interview Questions

**Q: What is DNS and why do we need it?**
A: DNS translates human-readable domain names to IP addresses. Without it, users would need to memorize IPs for every site.

**Q: Walk me through a full DNS lookup for github.com.**
A: Browser cache → OS cache → recursive resolver → root server → TLD server → authoritative server. Each layer caches the result per TTL.

**Q: What is TTL and why does it matter?**
A: TTL controls how long resolvers cache a DNS record. Low TTL = fast propagation of changes. High TTL = less DNS traffic. Critical for planning IP migrations.

**Q: What's the difference between an A record and a CNAME?**
A: A record maps a domain directly to an IPv4 address. CNAME maps a domain to another domain name. CNAMEs are useful so multiple aliases track one A record automatically.

**Q: How does Netflix route users to the nearest server using DNS?**
A: Netflix's authoritative DNS server inspects where the resolver request comes from and returns a different IP based on geography, directing users to the nearest CDN node.
