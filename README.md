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








```text
grpOriginalContent
├── btnThemeTrigger
├── imgThemeToggle
├── List_Icon
├── Grid_Icon
├── btnListBackground
├── btnGridBackground
├── btnGridListToggleContainer
├── divided_head
├── tmrPulse
├── btnInvisibleTrigger
├── icoChevron
├── btnCircleBackground
├── SavingMessage
├── Container1
│   ├── optimal_icon
│   ├── ausverkauft_icon
│   ├── niedrig_icon
│   ├── gesamt_icon
│   ├── btnOptimalTransparentTrigger
│   ├── btnSoldOutTransparentTrigger
│   ├── btnLowTransparentTrigger
│   ├── btnTotalTransparentTrigger
│   ├── lblNumberOptimal
│   ├── lblTitleOptimal
│   ├── lblNumberAusverkauft
│   ├── lblTitleAusverkauft
│   ├── lblNumberNiedrig
│   ├── lblTitleNiedrig
│   ├── lblNumberGesamt
│   ├── lblTitleGesamt
│   ├── btnOptimalBadge
│   ├── btnSoldOutBadge
│   ├── btnLowBadge
│   ├── btnTotalBadge
│   ├── btnOptimalRectangle
│   ├── btnSoldOutRectangle
│   ├── btnLowRectangle
│   └── btnTotalRectangle
├── Spinner1
├── LoadingOverlay
├── ddCategory
├── ddSystem
├── sharepoint_Image1
├── NumberOfItems
├── SearchIcon1
├── TextSearchBox1
├── lblAppName1
├── IconResetFilters
├── RectQuickActionBar1
└── BrowseGallery1
    ├── btnPulsatingDotList
    ├── btnArticleTitle
    ├── lblSKU_system_Pill_List
    ├── IconCopy_ArtNr_list
    ├── lblListLocation
    ├── lblSKUPill
    ├── btnListPlus
    ├── btnListMinus
    ├── txtListQty
    ├── btnStockQuantityBadge
    ├── lblListTitle
    ├── btnListStatusIndicator
    ├── btnListRowBackground
    ├── divider_card
    ├── IconCopy
    ├── IconCopy_1
    ├── btnBackgroundMinusTransparentTrigger
    ├── Icon_Minus_Grid
    ├── btnBackgroundMinus
    ├── btnOutLabel
    ├── btnInLabel
    ├── btnBackgroundPlusTransparentTrigger
    ├── Icon_Plus_Grid
    ├── btnPulsatingDotGrid
    ├── TxtInput1
    ├── btnBackgroundPlus
    ├── btnStockCountLabel_Grid
    ├── Subtitle1
    └── btnCardBackground


    ```
