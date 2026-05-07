# 01d — What is Hosting

## Key Concepts

| Term | Definition |
|------|-----------|
| Hosting | Renting server infrastructure to run your app on the internet |
| Shared Hosting | Multiple apps share one server — cheapest, lowest isolation |
| VPS | Virtual Private Server — isolated slice of a physical server |
| Dedicated Server | You own the entire physical machine |
| Cloud Hosting | On-demand servers from AWS/Azure/GCP — pay-as-you-go |
| PaaS | Platform as a Service — provider manages server, you deploy code |
| Auto-scaling | Automatically adding/removing server instances based on traffic |
| Elasticity | Ability to scale up AND back down automatically |
| Uptime | % of time server is available (99.9% = ~8.7 hrs downtime/year) |
| SLA | Service Level Agreement — uptime guarantee from provider |
| Region | Physical data center location (e.g., AWS `us-east-1`, `ap-south-1`) |

## Hosting Types Compared

| Type | Isolation | Scalability | Cost | Best For |
|------|-----------|-------------|------|----------|
| Shared | Low | None | Cheapest | Blogs, personal sites |
| VPS | Medium | Manual | Low-mid | Small-medium apps |
| Dedicated | High | Manual | High | Predictable high traffic |
| Cloud | High | Automatic | Pay-per-use | Startups to enterprise |
| PaaS | Managed | Automatic | Mid | Fast deployments |

## How Auto-Scaling Works

```
Traffic spike detected
        ↓
Auto Scaling Group spins up new instances (scale-out)
        ↓
Traffic normalizes
        ↓
Extra instances terminated (scale-in)
        ↓
You only pay for what was used
```

## Industry Examples

- **Netflix** — fully on AWS; multiple regions so if one goes down, traffic shifts automatically
- **Airbnb** — migrated from own servers to AWS, reduced ops overhead dramatically
- **Stack Overflow** — dedicated servers; predictable traffic makes it more cost-effective than cloud at their scale

## Interview Questions

**Q: What is web hosting?**
A: Renting server infrastructure to run your application so it's accessible over the internet.

**Q: What are the types of hosting?**
A: Shared, VPS, Dedicated, Cloud, and PaaS — each trades isolation and scalability against cost.

**Q: Why did the industry move to cloud hosting?**
A: Elasticity — auto-scale up during traffic spikes, scale back down after. Pay only for what you use.

**Q: When would you choose dedicated over cloud?**
A: When traffic is large but predictable — at scale, owning hardware is cheaper than paying cloud per-hour rates (e.g., Stack Overflow).

**Q: What is elasticity in cloud hosting?**
A: The ability to automatically scale servers up during spikes and back down when traffic drops.
