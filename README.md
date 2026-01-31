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



```
**Reviewing Current Stage**

I am now reviewing the current stage, making sure I am aligned with the agreed-upon "Execution Plan". Input hygiene is assumed to be complete. Next, I need to focus on identifying the next step.


**Focusing on Per-Line Logic**

I'm now zeroing in on Step 3, the per-line issue logic. This refactor of "Wareneingang prüfen" demands a shift from global checks to item-specific checks within Flow 2. My goal is to add a "Problem melden" toggle/button for each row, revealing "Beschädigt" checkbox and a text input for notes, storing this item state. I need to update the save handler to incorporate these new flags, while being careful to maintain the quantity data, protect FULL_INVENTORY, and to keep the portal logic working.
I've started drafting the prompt with this refactoring as the core context, focusing on the UI additions and how they will display in the expanded or modal row options.


**Optimizing UI Presentation**

I'm now refining the UI presentation, considering that the table is becoming crowded. A popover approach for quick entry of issue details seems preferable to directly embedding the fields in the row. My immediate focus is on suggesting a "Toggle" button, possibly a warning icon, that expands to show the popover when clicked. This approach directly addresses the user's initial requirement of an "Expand button per row labeled 'Problem melden'" without disrupting the layout. I am also working on Tasks 2-4 and polishing final details.



```markdown
