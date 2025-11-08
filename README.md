# Export Viewer

Simple web application for viewing and searching categorized export data.

## Files

- **index.html** - Standalone HTML viewer application with embedded CSS and JavaScript
- **export.json** - JSON data file containing categorized items with IDs and quantities

## Usage

1. Open `index.html` in a web browser
2. Click "Vali JSON fail" (Select JSON file) to load a JSON file
3. Browse categories and click on any key (e.g., P1, N2, M3) to view items
4. Use the search box to find items by ID - matching keys will be highlighted

## Data Format

The JSON file should contain an object with keys in the format `[Category][Number]` (e.g., P1, N2, M3), where each key contains an array of items:

```json
{
  "P1": [
    {"id": "111.222.33", "qty": ""}
  ],
  "N1": [
    {"id": "205.103.93", "qty": ""}
  ]
}
```

Each item has:
- `id` - Item identifier
- `qty` - Quantity (optional)

