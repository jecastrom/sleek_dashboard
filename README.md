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

```markdown
This is a very detailed and professional feedback loop. You have identified exactly the kind of UX/UI nuances that make an app feel "real" versus just a prototype.

Your feedback makes perfect sense. I can picture exactly where these changes go.

Here is the **Execution Plan** to apply these changes without breaking the app. We will group them by "Logic Type" rather than just Flow, to keep the code changes isolated.

### The Execution Plan

1.  **Fixing the Inputs (Flow 1):** We will fix the "Leading Zero" annoyance and the negative numbers.
2.  **The "Math" Columns (Flow 2 & 3):** We will add the "Offen" (Under-delivered) and "Überliefert" (Over-delivered) columns. This involves visual changes and math, but not deep logic yet.
3.  **The "Issue" Logic (Flow 2):** **(This is the big one)**. We will move the "Damage/Wrong Item" logic from the global Step 3 checkbox to the specific *lines* in Step 2. This requires updating the state to store comments/flags *per item*.
4.  **Detail View Headers (Flow 3):** Simple UI text update to show PO + Delivery Note.
5.  **The PDF Engine (Flow 1):** We will switch from text file generation to a real PDF library structure.

---

### Step 1: Input Hygiene (Flow 1)
Let's fix the annoying number behavior first.

**Prompt 1:**
> "I need to improve the UX of the **Quantity Input** fields in the **'Bestellung erstellen' (Flow 1, Step 2)**.
>
> **The Issues:**
> 1.  **Leading Zeros:** Currently, if I type a number, it keeps a leading zero (e.g., '014'). It should strip the leading zero automatically.
> 2.  **Negative Numbers:** The field allows negative numbers via the arrow keys.
>
> **The Task:**
> Update the Input component logic for the Quantity field:
> *   **Sanitization:** When the value changes, parse it to an Integer immediately (`parseInt(val, 10)`). If the result is NaN, default to 0. This removes leading zeros.
> *   **Constraints:** Set `min="0"` on the input. Prevent the user from typing a minus sign.
>
> **CRITICAL WARNINGS:**
> *   **CONSTRAINT 1:** Apply this ONLY to the Quantity input in the Create Order wizard.
> *   **CONSTRAINT 2:** Do not break the 'Add Item' logic."

---

### Step 2: The "Math" Columns (Flow 2)
Now let's add the calculation columns to the Verification table.

**Prompt 2:**
> "I need to upgrade the **Verification Table** in **'Wareneingang prüfen' (Flow 2, Step 2)**.
>
> **The Task:**
> Update the table columns to give better visibility on quantities.
>
> **1. New Columns:**
> The table currently has 'Ordered' (Bestellt) and 'Received' (Geliefert). Add two new **Read-Only** columns to the right of 'Received':
> *   **'Offen' (Under-delivered):** Calculate `Ordered - Received`.
>     *   Display ONLY if result > 0.
>     *   If result > 0, style the text in **Orange** (indicating pending items).
>     *   If result <= 0, leave empty or show a dash '-'.
> *   **'Zu viel' (Over-delivered):** Calculate `Received - Ordered`.
>     *   Display ONLY if result > 0.
>     *   If result > 0, style the text in **Red** (indicating error).
>
> **2. Visual Update:**
> Ensure the table fits these new columns (adjust column widths if necessary).
>
> **CRITICAL WARNINGS:**
> *   **CONSTRAINT 1:** Do not change the logic of the 'Received' input field (it must remain editable).
> *   **CONSTRAINT 2:** This is a visual/calculation update only. Do not change the save logic yet."

---

### Step 3: The "Per-Line" Issue Logic (Refactoring Flow 2)
This moves the damage check from the end (Global) to the middle (Per Item). This is a smart move for data accuracy.

**Prompt 3:**
> "We are refactoring the **Damage/Issue Reporting** logic in **'Wareneingang prüfen' (Flow 2)**.
>
> **The Goal:**
> Move the error reporting from a global checkbox in Step 3 to a **Per-Item** reporting system in Step 2.
>
> **1. Step 2 UI Updates (The Table):**
> *   Add a new column or an 'Expand' button per row labeled **"Problem melden"**.
> *   When clicked/activated, show two fields for that specific item:
>     *   **Checkbox:** "Beschädigt" (Damaged).
>     *   **Text Input:** "Falsch geliefert / Bemerkung" (Wrong product / Notes).
>
> **2. Step 3 UI Updates (Cleanup):**
> *   **Remove** the global "Ware beschädigt" checkbox from Step 3 entirely. The user should have already flagged items in Step 2.
>
> **3. Logic Updates (Status Calculation):**
> *   Update the `handleSave` status logic.
> *   **New Rule:** If **ANY** item in the list has the "Beschädigt" flag checked OR has text in the "Falsch geliefert" field $\rightarrow$ Set the Global Status to **"Falsch geliefert"** (or 'Reklamation').
> *   (Keep the existing Teillieferung/Überlieferung logic for quantity mismatches).
>
> **CRITICAL WARNINGS:**
> *   **CONSTRAINT 1:** Do not lose the 'Received Quantity' data when toggling the 'Problem' fields.
> *   **CONSTRAINT 2:** Ensure the Status Calculation checks the *items array* for flags."

---

### Step 4: Detail View Cleanup (Flow 3)
Updating the "Management" view to match the new data richness.

**Prompt 4:**
> "I need to update the **Receipt Details View** in the **Management Dashboard (Flow 3)**.
>
> **1. Header Update:**
> *   In the details card header, currently it might only show the Receipt Number.
> *   Update it to clearly display **BOTH**:
>     *   **"Lieferschein: [Number]"**
>     *   **"Bestellung: [PO Number]"** (if available).
>
> **2. 'Positionen' Tab Update:**
> *   Update the items table in this tab to match the layout we just built for Flow 2.
> *   Add the **'Offen' (Under)** and **'Zu viel' (Over)** calculated columns here as well, so the admin can see exactly what happened.
>
> **CRITICAL WARNINGS:**
> *   **CONSTRAINT 1:** Read-only mode. Do not make these fields editable in the Management view.
> *   **CONSTRAINT 2:** Maintain the existing Portal/Tab architecture."

---

### Step 5: PDF Generation (Flow 1)
Since we cannot "upload" a template to the AI code builder easily, we will ask it to use a library to draw a professional PDF.

**Prompt 5:**
> "I need to upgrade the **PDF Generation** in **'Bestellung erstellen' (Flow 1)**.
>
> **The Task:**
> Replace the simple text-file generation with a proper **PDF Generator** using a library like `jspdf` (or a built-in browser print-to-pdf technique if you prefer).
>
> **1. The PDF Layout:**
> The generated PDF should look like a professional Purchase Order:
> *   **Header:** 'BESTELLUNG' (Large bold text).
> *   **Info Block:** Order Number, Date, Supplier Name.
> *   **Table:** A clean table listing: SKU | Article Name | Quantity.
> *   **Footer:** 'Generiert von Inventar Pro'.
>
> **2. Implementation:**
> *   Update the `generatePOPdf` function.
> *   Instead of creating a `.txt` blob, create a `.pdf` blob.
>
> **CRITICAL WARNINGS:**
> *   **CONSTRAINT 1:** Ensure the PDF download trigger in Step 3 still works.
> *   **CONSTRAINT 2:** Do not break the 'Save' logic."

***

**My Suggestion:**
Start with **Prompt 1 (Inputs)** and **Prompt 2 (Math Columns)**. These are safe, visual changes that will immediately make the app feel better.
Then we can tackle **Prompt 3 (Per-Line Logic)**, which is the most complex one.

Shall we begin with Prompt 1?
