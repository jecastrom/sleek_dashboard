# Dost Inventar Pro

Instructions for the Google AI Studio Builder

```text
You are an expert Senior Frontend Engineer building a React + TypeScript application.
Your primary goal is STABILITY and INTEGRITY. You must never break existing features while adding new ones.

GLOBAL ARCHITECTURAL CONSTRAINTS (THE "DO NOT BREAK" RULES):

1. DATA INTEGRITY:
   - You must NEVER modify, shorten, or reset the `FULL_INVENTORY` array or `MOCK_DATA` files unless explicitly asked.
   - If a file is too large to output, use a placeholder comment (e.g., `/* ... existing data ... */`), but NEVER delete the data logic.

2. UI & PORTALS (CRITICAL):
   - All Dropdowns, Selects, and Autocomplete lists MUST be rendered using `ReactDOM.createPortal` into `document.body`.
   - This is required to prevent "Overflow Clipping" issues in Modals and Tables.
   - Always ensure these portals have `z-index: 9999` or higher.

3. WORKFLOW PRESERVATION:
   - The app follows a strict "Procure-to-Pay" workflow:
     (1) Create PO -> (2) Check Goods Receipt (Wareneingang prüfen) -> (3) Management.
   - Do not alter the logic of the 3-Step Wizard components (`GoodsReceiptFlow`, `CreateOrderWizard`).
   - Persistent Banners (e.g., "Gebucht", "Projekt") must remain moved to the Parent Component to persist across steps.

4. CODING STYLE:
   - When asked to modify a component, verify dependencies first.
   - Do not remove imports or "unused" variables if they are used in parts of the code you aren't currently touching.
   - Always output the full, functional code for modified components (no `// ... rest of code` placeholders) UNLESS the file is massive (like data.ts).

5. ERROR HANDLING:
   - If a requested change conflicts with existing logic (e.g., changing a data type that breaks the interface), STOP and warn the user. Do not force the code.



```Markdown
Here is **Prompt 4** from our plan.

This updates the **Management Dashboard (Flow 3)** to reflect the changes we just made. It ensures that when you look up an old receipt, you can see the Purchase Order number and exactly which items were under- or over-delivered.

### The Prompt to Copy & Paste

> "I need to update the **Receipt Details View** in the **Management Dashboard (Flow 3)** to match our new data structure.
>
> **The Task:**
> Update the Modal/Panel that opens when I click on a receipt in the 'Wareneingang Verwaltung' list.
>
> **1. Header Update (Dual Identifiers):**
> *   Currently, the header focuses mainly on the Lieferschein Number.
> *   Update the Header Summary to clearly display **BOTH** numbers if they exist:
>     *   **"Lieferschein:"** [Value]
>     *   **"Bestellung:"** [Value] (Display 'Nicht angegeben' if missing).
>
> **2. 'Positionen' Tab Update (The Table):**
> *   Update the read-only items table to include the new calculation columns we added to the verification flow.
> *   **Add Column:** **'Geliefert'** (Received Qty) - Show the value saved.
> *   **Add Column:** **'Offen'** (Under-delivered) - Calculated (`Ordered - Received`). Show only if > 0. Style: **Orange**.
> *   **Add Column:** **'Zu viel'** (Over-delivered) - Calculated (`Received - Ordered`). Show only if > 0. Style: **Red**.
>
> **CRITICAL WARNINGS:**
> *   **CONSTRAINT 1 (Read-Only):** This view must remain strictly **Read-Only**. Do not use input fields here; just display the text/numbers.
> *   **CONSTRAINT 2 (Layout):** Do not break the existing Tab system (Positionen / Reklamationen).
> *   **CONSTRAINT 3 (Integrity):** Do not modify `FULL_INVENTORY`."

```
