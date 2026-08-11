---
name: Apex Trigger Best Practices
description: Learn the fundamental best practices for writing Apex triggers in Salesforce.
---

# Apex Trigger Best Practices

When developing Apex triggers in Salesforce, it's essential to follow best practices to ensure your code is scalable, maintainable, and performs well within Salesforce governor limits. Here are 4 basic best practices:

1. **One Trigger Per Object**
   Having a single trigger per object gives you complete control over the order of execution. If you have multiple triggers on the same object, Salesforce does not guarantee the order in which they run.

2. **Logic-less Triggers**
   Triggers should not contain any logic. Instead, they should delegate the logic to a handler class. This makes your code more reusable, easier to test, and simplifies maintenance.

3. **Context-Specific Handler Methods**
   Create context-specific handler methods in your trigger handler (e.g., `onBeforeInsert`, `onAfterUpdate`). This keeps the trigger clean and delegates the specific execution context logic clearly.

4. **Bulkify Your Code**
   Always assume that triggers might process more than one record at a time (e.g., during a data load or bulk API operation). Never use DML operations or SOQL queries inside a `for` loop.
