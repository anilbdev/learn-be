# 01c — Domain Names

## Key Concepts

| Term | Definition |
|------|-----------|
| Domain Name | Human-readable alias that maps to an IP address |
| TLD | Top-Level Domain — `.com`, `.org`, `.io` — managed by ICANN |
| SLD | Second-Level Domain — the name you register (e.g., `google`) |
| Subdomain | Prefix you control without extra registration (e.g., `mail`, `api`) |
| ICANN | Global body that oversees domain name system |
| Registrar | Company where you buy/register domains (GoDaddy, Namecheap) |
| Name Server | Tells the internet where your domain's DNS records live |
| GeoDNS | Returns different IP for same domain based on user's location |
| Anycast | Routes user to nearest server using same IP/domain |

## How It Works

```
https://mail.google.com
         ↑    ↑      ↑
       sub  second  top-level
      domain domain  domain
```

1. You register a domain via a **Registrar** (first-come, first-served)
2. Registrar records your ownership with **ICANN**
3. You point domain to **Name Servers**
4. Name Servers hold **DNS records** mapping domain → IP
5. Browser queries DNS → gets IP → connects to server

## Industry Examples

- **Netflix** — uses `api.netflix.com`, `assets.netflix.com` to route traffic to different servers independently
- **Google** — `google.com` returns different IPs based on location (GeoDNS) for nearest server
- **Nike** — ICANN's UDRP process lets trademark holders reclaim misused domains

## Interview Questions

**Q: What is a domain name?**
A: A human-readable alias for an IP address, managed via ICANN and purchased through registrars.

**Q: What are the parts of a domain name?**
A: TLD (`.com`), SLD (`google`), and optionally a subdomain (`mail`, `api`).

**Q: Who controls domain names globally?**
A: ICANN oversees the system; registrars sell and manage individual domains.

**Q: Can one domain point to multiple IPs?**
A: Yes — GeoDNS returns different IPs based on user location, routing them to the nearest server.

**Q: What happens if two companies want the same domain?**
A: First-come, first-served. Trademark holders can dispute via ICANN's UDRP process.
