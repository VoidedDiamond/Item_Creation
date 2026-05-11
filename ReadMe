# P21 Item Creator

Internal tool for Hydraquip. Provides a guided, validated form for creating new items in Prophet 21 - enforcing supplier-specific item ID format rules and surfacing potential duplicates before a record is created.

---

## How It Works

1. User selects a supplier from the dropdown
2. The form loads that supplier's item ID format rule and product group list
3. Item ID is validated in real time against the supplier's pattern
4. Before submission, the form checks P21 for existing items with a similar ID (fuzzy match)
5. If duplicates are found, user must confirm before proceeding
6. On submit, the validated record is posted to P21 via REST API

---

## Files

| File | Purpose |
|---|---|
| `index.html` | The form UI - open this in a browser to run locally |
| `suppliers.json` | Supplier config: ID format rules and product group lists |

---

## Running Locally

No build step required. Open `index.html` directly in a browser. The form loads `suppliers.json` from the same folder.

> Note: If you open via `file://` protocol, some browsers block local JSON fetch. Use a simple local server instead:
> ```
> npx serve .
> ```
> Then open `http://localhost:3000`

---

## P21 API Setup (when ready)

Two stubs in `index.html` need to be replaced with real P21 REST API calls:

### 1. Duplicate Check
Search for existing items with a similar item ID.

```js
// In checkDuplicates()
const res = await fetch(`${P21_API_BASE}/items?filter=item_id like '%${normalized}%'`, {
  headers: { 'Authorization': 'Bearer YOUR_TOKEN' }
});
const data = await res.json();
return data.value.map(i => i.item_id);
```

### 2. Item Create
POST the new item record.

```js
// In createItem()
const res = await fetch(`${P21_API_BASE}/items`, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer YOUR_TOKEN'
  },
  body: JSON.stringify(payload)
});
```

Update `P21_API_BASE` at the top of the script block in `index.html`:
```js
const P21_API_BASE = 'https://YOUR-P21-INSTANCE/api/v2';
```

---

## Adding More Suppliers

Edit `suppliers.json`. Each entry follows this structure:

```json
{
  "supplier_id": "106149",
  "supplier_name": "Bosch-Rexroth Corporation",
  "pattern_display": "RXXXXXXXXX (e.g. R902401166)",
  "pattern_hint": "Must start with R followed by exactly 9 digits",
  "regex": "^R[0-9]{9}$",
  "product_groups": [
    { "id": "BOSCH", "label": "BOSCH - Bosch Racine" }
  ]
}
```

No code changes needed - just add to the array and save.

---

## Suppliers Configured (v1)

| Supplier ID | Name | Format |
|---|---|---|
| 105111 | Danfoss Power Solutions II LLC - Aeroquip | `XXXXX-XX` |
| 106388 | Danfoss Power Solutions II LLC | 8-digit numeric |
| 106149 | Bosch-Rexroth Corporation | `R` + 9 digits |

---

## Planned / Next Steps

- Wire in P21 REST API (duplicate check + item create)
- Add proxy function to keep API credentials server-side
- Expand supplier list beyond initial 3
- Add unit of measure field
- Add location/branch assignment
