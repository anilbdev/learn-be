# 04c — SOAP

## Key Concepts

| Term | Definition |
|------|-----------|
| SOAP | Simple Object Access Protocol — strict XML-based protocol for APIs |
| WSDL | Web Services Description Language — mandatory XML contract describing the service |
| Envelope | Wrapper element of every SOAP message |
| Fault | SOAP's built-in error structure (inside Body) |
| WS-Security | Built-in SOAP standard for message-level security |

---

## SOAP vs REST

| | SOAP | REST |
|--|------|------|
| Type | Protocol — strict rules | Architectural style — flexible |
| Data format | XML only | JSON, XML, anything |
| Transport | HTTP, SMTP, TCP | HTTP only |
| Contract | WSDL (mandatory) | OpenAPI (optional) |
| Error handling | Built-in Fault structure | HTTP status codes |
| Speed | Slower — verbose XML | Faster — lightweight JSON |
| Use case | Enterprise, banking, legacy | Web, mobile, modern APIs |

---

## SOAP Message Structure

```xml
<soap:Envelope>
  <soap:Header>
    <!-- auth tokens, metadata, routing -->
  </soap:Header>
  <soap:Body>
    <GetUserRequest>
      <UserId>1</UserId>
    </GetUserRequest>
  </soap:Body>
</soap:Envelope>
```

| Part | Purpose |
|------|---------|
| Envelope | Marks it as a SOAP message |
| Header | Optional — auth, routing, transaction info |
| Body | Actual request or response data |
| Fault | Error details when something fails |

---

## Why SOAP Still Exists

- **Strict contracts** — WSDL guarantees both sides agree on exact structure
- **Built-in standards** — WS-Security, WS-ReliableMessaging, WS-Transaction
- **Legacy systems** — rewriting 20-year-old banking infrastructure is too risky and expensive
- **Audit trails** — XML messages are easy to log and verify for compliance

---

## Industry Examples

| Company/System | Usage |
|---------------|-------|
| **PayPal** | Older APIs were SOAP, now REST but SOAP still supported |
| **Salesforce** | Still offers SOAP API alongside REST |
| **Banking/SWIFT** | Interbank communication uses SOAP-based services |
| **Government APIs** | Older integrations (e.g. GST) are SOAP-based |

---

## Interview Questions

**Q: What is SOAP and how does it differ from REST?**
A: SOAP is a strict protocol using XML only and requiring a WSDL contract. REST is a flexible style using JSON over HTTP. SOAP is heavier but has built-in security and transaction standards.

**Q: What is WSDL?**
A: Web Services Description Language — an XML file that acts as a mandatory contract, describing what operations the service offers, what parameters they take, and where the service lives.

**Q: Why do enterprises still use SOAP?**
A: Built-in WS-Security and transaction standards, strict contracts for compliance, and legacy systems too expensive to rewrite. Not preference — cost of change.
