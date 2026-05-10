# 05c — CDN (Content Delivery Network)

## Key Concepts

- **CDN** — a network of geographically distributed servers that cache content close to users
- **Edge node / Edge server** — a CDN server in a specific geographic location
- **Origin server** — your actual backend server where content originates
- **Cache purging** — forcefully deleting cached content from CDN before TTL expires
- **Cache versioning** — changing the URL when content changes so CDN treats it as a new file

---

## How It Works

```
Without CDN:  Mumbai User → Virginia Server (~13,000 km, ~200ms latency)

With CDN:     Mumbai User → CDN Edge Node in Mumbai (~5ms latency)
                                      ↑
                              Cached origin content
```

1. User requests content
2. Request hits nearest CDN edge node
3. **Cache hit** → served immediately from edge
4. **Cache miss** → edge fetches from origin, caches it, serves it
5. Next user in same region → cache hit

---

## What CDNs Cache

- Static assets: images, CSS, JS, fonts, videos
- API responses (if `Cache-Control: public`)
- Entire HTML pages (static sites)
- **Not cached:** dynamic, personalized content (user feeds, dashboards)

---

## CDN Cache Invalidation Strategies

| Strategy | How | When to Use |
|----------|-----|-------------|
| **TTL expiry** | Cache auto-expires after set time | Routine updates |
| **Cache purge** | API call to delete specific cached files | Urgent updates |
| **Cache versioning** | Change URL when content changes (hash in filename) | Best practice for assets |
| **Surrogate keys / Cache tags** | Tag objects, purge by tag | One change affects many URLs |

**The rule:** TTL handles routine expiry. Purging handles urgent updates. Versioning avoids the problem entirely.

---

## Popular CDNs

| CDN | Used By |
|-----|---------|
| Cloudflare | Millions of websites |
| AWS CloudFront | Amazon ecosystem |
| Akamai | Enterprise, streaming |
| Fastly | GitHub, Stripe, Reddit |

---

## Industry Examples

- **Netflix** — video files cached at edge nodes in 190+ countries; stream comes from ~50km away
- **Hotstar** — 25M concurrent IPL viewers served via CDN; origin server would have collapsed
- **GitHub** — JS/CSS on Fastly; fast globally despite single origin

---

## Interview Questions

**Q: What is a CDN and why use one?**
> A network of geographically distributed servers that cache content near users. Reduces latency and offloads traffic from the origin server.

**Q: What happens when origin content changes?**
> Cached copies remain until TTL expires. For urgent updates, use cache purging via CDN API. Best practice is cache versioning — change the URL when content changes.

**Q: What does a CDN cache?**
> Mainly static assets (images, JS, CSS, video). Dynamic or personalized content typically bypasses the CDN.

**Q: What is cache versioning?**
> Embedding a hash or version number in the asset URL. When the file changes, the URL changes — CDN treats it as a brand new file and fetches fresh from origin.

**Q: What are surrogate keys?**
> Cache tags that group related cached objects. Purging a tag clears all objects associated with it — useful when one DB change affects many cached URLs.
