---
name: Create LWC Component
description: Best practices for Lightning Web Components (LWC) including state management, wire service, and UI/UX standards.
status: certified
---

# Lightning Web Component (LWC) Standards

## 1. File Structure and Naming
- Use `kebab-case` for component folder names (e.g., `account-viewer`).
- Keep components small and focused on a single responsibility.
- Use a `constants.js` file if your component requires many magic strings, API names, or state keys.

## 2. Data Fetching and Wire Service
- Prefer the `@wire` adapter over imperative Apex calls when you only need to read data. The wire service provisions data directly to the component and integrates with the Lightning Data Service (LDS) cache.
- Use imperative Apex calls for DML operations (create, update, delete) or when data needs to be fetched programmatically on a button click.

**Example Wire:**
```javascript
import { LightningElement, wire } from 'lwc';
import getAccounts from '@salesforce/apex/AccountController.getAccounts';

export default class AccountViewer extends LightningElement {
    @wire(getAccounts) accounts;

    get hasAccounts() {
        return this.accounts.data && this.accounts.data.length > 0;
    }
}
```

## 3. DOM Manipulation
- **NEVER** manipulate the DOM directly using `document.querySelector` or `document.getElementById` to reach outside the component.
- Use `this.template.querySelector` to access elements within your component's Shadow DOM.
- Rely on data binding and reactive properties (`@track` for complex objects in older API versions, though modern LWC reacts to top-level object changes automatically) instead of manual DOM updates.

## 4. Communication Between Components
- **Parent to Child:** Pass data via public properties decorated with `@api`.
- **Child to Parent:** Use standard DOM events via `CustomEvent`.
- **Unrelated Components:** Use the Lightning Message Service (LMS) to publish and subscribe to messages across the DOM, Aura, and Visualforce boundaries.

## 5. Error Handling
- Wrap imperative Apex calls in `try/catch` blocks.
- Always provide user-facing error feedback. Use the `ShowToastEvent` to display error messages elegantly in the Lightning Experience.
- When dealing with wired data, ensure you check `if (error)` and display an appropriate UI fallback.
