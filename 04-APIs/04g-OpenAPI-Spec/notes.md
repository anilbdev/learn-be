# 04g — OpenAPI Specification

## Key Concepts

| Term | Definition |
|---|---|
| OpenAPI Spec | A standard file (JSON/YAML) that fully describes a REST API — endpoints, parameters, request/response schemas |
| Swagger | Original name; now refers to the tooling (Swagger UI, Swagger Editor). OpenAPI is the spec standard (v3.0+) |
| Contract-first design | Write the OpenAPI spec before writing any code; teams agree on the contract upfront |
| Swagger UI | Interactive documentation auto-generated from the OpenAPI spec |
| openapi-generator | Tool that generates client SDKs in multiple languages from a spec file |

---

## How It Works

**Contract-First Flow:**
1. Frontend + backend teams agree on endpoints, parameters, and response shapes
2. Agreement is written into an OpenAPI spec document (YAML or JSON)
3. Frontend team mocks against the spec (using Postman, Swagger UI, or mock servers)
4. Backend team implements the real APIs against the same spec
5. When backend is ready, frontend replaces mock URLs with real ones — no surprises

**In .NET:**
- Swashbuckle auto-generates the OpenAPI spec from your controllers and attributes at startup
- Swagger UI is served from this generated spec (interactive docs out of the box)
- Request validation against the spec is NOT on by default — requires explicit middleware
- Data Annotations (`[Required]`, `[Range]`) handle validation but are not spec-driven

**Request flow (default .NET):**
```
Request → Routing → Model Binding → Data Annotation Validation → Controller
```

**With spec-based validation middleware added:**
```
Request → OpenAPI Validation Middleware → Routing → Controller
```

---

## Industry Examples

- **Stripe** — publishes full OpenAPI spec; third-party SDKs and tools are auto-generated from it
- **GitHub** — REST API documented via OpenAPI; client libraries in 6+ languages generated from the spec
- **AWS API Gateway** — accepts an OpenAPI spec to auto-configure routes, auth, and validation

---

## Interview Questions

**Q: What is OpenAPI Spec?**
A: A standard JSON/YAML file that describes a REST API's endpoints, parameters, and response schemas — used to generate docs, client SDKs, and enable contract-first design.

**Q: What's the difference between OpenAPI and Swagger?**
A: Swagger was the original name. OpenAPI is the standardized open spec (v3.0+, Linux Foundation). Swagger now refers to the tooling — Swagger UI, Swagger Editor.

**Q: What is contract-first API design?**
A: Both teams agree on the OpenAPI spec before writing code. Frontend mocks against the spec; backend implements it. They work in parallel and integrate at the end.

**Q: How does OpenAPI help teams work in parallel?**
A: The spec acts as a shared contract. Frontend uses mock servers (Postman, Swagger UI) based on the spec. Backend builds the real API. When done, frontend swaps mock for real — no integration surprises.

**Q: Does .NET auto-validate requests against the OpenAPI spec?**
A: No — Swashbuckle generates the spec and serves Swagger UI, but spec-based request validation requires explicit middleware. Standard .NET uses Data Annotations for validation.
