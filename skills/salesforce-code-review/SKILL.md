---
name: Salesforce Code Review
description: Guidelines for reviewing Apex and Lightning Web Component (LWC) code for quality, security, and maintainability.
tags:
  - apex
  - lwc
  - code-review
  - best-practices
status: certified
---

# Salesforce Code Review

Use these guidelines when reviewing Apex classes, triggers, and Lightning Web Components to ensure consistent quality, security, and maintainability across the codebase.

---

## Guideline 1 — Apex: Bulkify All Logic

- Every Apex method and trigger must handle collections, never single records.
- Use `Map`, `Set`, and `List` structures for SOQL and DML operations to avoid governor limit violations.
- A single SOQL query inside a `for` loop is an **immediate review blocker**.

```apex
// ✅ Correct — bulkified
List<Account> accounts = [SELECT Id, Name FROM Account WHERE Id IN :accountIds];

// ❌ Wrong — SOQL inside loop
for (Id accId : accountIds) {
    Account a = [SELECT Id FROM Account WHERE Id = :accId];
}
```

---

## Guideline 2 — Apex: Enforce Security and CRUD/FLS Checks

- All SOQL queries in Apex classes (not triggers) should use `WITH USER_MODE` or `WITH SECURITY_ENFORCED` unless there is an explicit, documented reason to bypass.
- Never expose sensitive fields (e.g., SSN, passwords) through SOQL results returned to the client.
- Validate user permissions before performing DML in service classes.

```apex
// ✅ Enforces field-level security
List<Contact> contacts = [SELECT Id, Email FROM Contact WITH USER_MODE];
```

---

## Guideline 3 — LWC: No Direct DOM Manipulation; Use Reactive Properties

- Never use `this.template.querySelector` to set styles or toggle visibility. Use reactive properties bound to `class` or `lwc:if` directives instead.
- All external data must flow through `@wire` or explicit `@api` properties — avoid imperative Apex calls unless a reactive wire adapter is unsuitable.
- Confirm that every `connectedCallback` side-effect has a corresponding cleanup in `disconnectedCallback` to prevent memory leaks.

```js
// ✅ Reactive — no direct DOM
@track isVisible = false;
toggle() { this.isVisible = !this.isVisible; }

// ❌ Direct DOM manipulation
this.template.querySelector('.my-div').style.display = 'none';
```

---

## Guideline 4 — General: Test Coverage and Assertions

- Apex test classes must achieve **≥ 85% code coverage** with meaningful `System.assert` / `Assert.areEqual` statements — not just empty coverage runs.
- LWC Jest tests must cover the happy path and at least one error/edge-case per public `@api` method or event handler.
- Mock all Apex wire adapters in Jest tests; never make real server calls in tests.

```apex
// ✅ Meaningful assertion
System.assertEquals(1, result.size(), 'Expected exactly one record returned');

// ❌ Coverage-only, no assertion
CodeReviewService.doSomething(testData);
```
