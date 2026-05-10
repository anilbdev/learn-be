# 07c — Functional Testing

## Key Concepts

| Term | Definition |
|---|---|
| Functional Test | Verifies a feature works according to business requirements from user's perspective |
| Black Box | Tests only inputs and outputs — doesn't care about internal code |
| E2E Test | End-to-end test — often synonymous with functional testing |
| User Story | Business requirement that defines what functional tests should verify |
| Browser Automation | Tools (Selenium, Cypress, Playwright) that simulate real user interactions |

---

## How It Works

1. Define expected behavior from business requirements / user stories
2. Automate a real user flow through the UI or API
3. Assert the user-facing outcome (not internal state)
4. Run before releases — not on every commit

---

## The Three Testing Levels Compared

| | Unit | Integration | Functional |
|---|---|---|---|
| Perspective | Developer | Developer | User / Business |
| Tests | Code logic | Component connections | User-facing behavior |
| Dependencies | Mocked | Real | Real (full system) |
| Speed | Fastest | Moderate | Slowest |
| Written by | Developers | Developers | QA / Business Analysts |
| Pyramid position | Bottom (many) | Middle (some) | Top (few) |

---

## Testing Pyramid

```
        /\
       /  \
      / E2E \          ← Functional tests — few, slowest, highest confidence
     /--------\
    / Integration\     ← Some
   /--------------\
  /   Unit Tests   \   ← Many, fastest
 /------------------\
```

---

## Industry Examples

- **Uber** — functional test for ride booking: user logs in → enters destination → confirms → verifies driver assigned, ETA shown, fare displayed
- **Amazon** — functional test for checkout: add to cart → enter address → pay → verify order confirmation email received
- **Netflix** — functional test for playback: log in → select title → press play → verify video starts within 3 seconds

---

## Tools

| Tool | Notes |
|---|---|
| Selenium | Oldest, most widely supported browser automation |
| Cypress | Modern, developer-friendly, fast feedback |
| Playwright | Supports Chromium, Firefox, Safari — built by Microsoft |

---

## Interview Questions

**Q1: What is functional testing?**
Testing whether a feature meets business requirements from the user's perspective. Treats the system as a black box — only the outcome matters, not how the code works internally.

**Q2: How is functional testing different from unit and integration testing?**
Unit tests verify code logic in isolation. Integration tests verify components connect correctly. Functional tests verify the user gets the expected outcome — defined by business requirements, not code structure.

**Q3: Who writes functional tests?**
Typically QA engineers or business analysts. Test cases come from user stories and business requirements, not from reading the code.

**Q4: What tools are used for functional testing?**
Selenium, Cypress, and Playwright — they automate a real browser, simulating user clicks and interactions end-to-end.

**Q5: Where does functional testing sit in the Testing Pyramid?**
At the top — fewest in number, slowest to run, but highest confidence that the product works from a user's perspective.
