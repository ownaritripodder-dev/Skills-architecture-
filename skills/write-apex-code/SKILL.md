---
name: Write Apex Code
description: Architectural standards, best practices, and trigger frameworks for writing robust Apex code.
status: certified
---

# Apex Coding Standards & Best Practices

## 1. One Trigger Per Object Rule
**Mandatory:** Never create multiple triggers for a single sObject.
- Implement a single trigger per object and delegate the logic to a dedicated handler class.
- *Why?* Multiple triggers on the same object do not guarantee the order of execution, which can lead to unpredictable behaviors and recursive loops.

**Example Trigger Pattern:**
```apex
trigger AccountTrigger on Account (before insert, before update, after insert, after update) {
    AccountTriggerHandler handler = new AccountTriggerHandler();
    if (Trigger.isBefore && Trigger.isInsert) {
        handler.onBeforeInsert(Trigger.new);
    }
    // ... handle other contexts
}
```

## 2. Bulkification
Always design your Apex code to handle bulk data. 
- Never perform DML operations or SOQL queries inside a `for` loop.
- Always collect records into a List or Set and perform DML/SOQL on the collection outside the loop.

**Bad Practice:**
```apex
for (Account acc : Trigger.new) {
    insert new Contact(AccountId = acc.Id, LastName = 'Smith'); // DO NOT DO THIS
}
```

**Good Practice:**
```apex
List<Contact> contactsToInsert = new List<Contact>();
for (Account acc : Trigger.new) {
    contactsToInsert.add(new Contact(AccountId = acc.Id, LastName = 'Smith'));
}
if (!contactsToInsert.isEmpty()) {
    insert contactsToInsert;
}
```

## 3. Test Classes
- Aim for at least 85% test coverage (Salesforce requires 75%, but we strive for higher).
- Test classes must use `@isTest`.
- Create a `TestDataFactory` class to generate mock data. Do NOT use `SeeAllData=true` unless absolutely necessary (e.g., when testing Pricebooks or specific metadata that cannot be mocked).
- Always use `System.runAs()` to test profile and permission set constraints.
- ALWAYS include System.assert() methods.
