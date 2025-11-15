# Export Viewer

Simple web application for viewing and searching categorized export data.

## Files

- **index.html** - StockFinder Map - interactive map view with area visualization (see details below) - **Homepage**
- **index1.html** - Standalone HTML viewer application with embedded CSS and JavaScript
- **index2.html** - StockFinder by Area - shows items grouped by area with sidebar navigation
- **export.json** - JSON data file containing categorized items with IDs and quantities
- **coordinates.json** - JSON file containing area coordinates for map visualization

## Usage

**For the map view (index.html - homepage):**
1. Open `index.html` in a web browser
2. The map view will automatically load on GitHub Pages, or use the file buttons for local development
3. Select areas from the sidebar to visualize them on the map

**For the standalone viewer (index1.html):**
1. Open `index1.html` in a web browser
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

## index.html - StockFinder Map (Homepage)

**Overview:**
Interactive map view that displays areas as rectangular boxes on a floor plan image. Users can select multiple areas simultaneously to visualize their locations on the map.

**Features:**
- Area selection with multi-select capability (toggle on/off)
- Visual representation of areas as rectangular boxes on the map
- Area boxes positioned using coordinates from `coordinates.json`
- Automatic mapping between area names (Area1-7) and coordinate keys (ABCD, EFGHi, etc.)
- Sidebar displays area names with coordinate keys: "Area1 (ABCD)" format
- Responsive design - boxes scale with image resize
- Support for area labels (text display in boxes)
- Wide sidebar (250px) to prevent text wrapping

**How it works:**

**GitHub Pages (Production):**
- Automatically loads `export.json` to get area names
- Automatically loads `coordinates.json` to get area coordinates
- No buttons visible - everything loads automatically

**Local Development (Windows PC):**
- Two buttons in the sidebar:
  1. **"Open export.json"** - Loads area names from `export.json` file
  2. **"Open coordinates.json"** - Loads area coordinates from `coordinates.json` file
- Use file dialogs to select the respective JSON files
- Both files need to be loaded for full functionality

**Usage:**
1. Load `export.json` to populate the area list in the sidebar (shows: Area1, Area2, Area3, etc.)
2. Load `coordinates.json` to enable area box visualization on the map
   - After loading, sidebar area names update to show coordinate keys: "Area1 (ABCD)", "Area2 (EFGHi)", etc.
   - The mapping is automatic: first coordinate key → Area1, second → Area2, etc.
3. Click area names in the sidebar to toggle their selection (multiple areas can be active)
4. Selected areas appear as rectangular boxes on the map image
5. Boxes are positioned using relative coordinates (%), so they scale correctly when the image is resized

**Coordinate System:**
- Coordinates are stored in `coordinates.json` with both absolute (px) and relative (%) values
- Relative coordinates are used for positioning to ensure boxes scale with the image
- Each area has: `x`, `y`, `width`, `height` (absolute) and `relX`, `relY`, `relWidth`, `relHeight` (relative)
- **Important:** The order of keys in `coordinates.json` must match the area order (Area1, Area2, Area3, etc.)
  - First key in `coordinates.json` → Area1
  - Second key → Area2
  - Third key → Area3
  - And so on...

**Debugging:**
- Open browser console (F12) to see detailed logs about:
  - Coordinate loading and mapping
  - Active area selection
  - Box creation and positioning
  - Image loading status

**Hidden Features (for development):**
- Draggable/resizable coordinate editor box (hidden by default)
- Coordinate display panel (hidden by default)
- Can be enabled by changing CSS `display: none` to `display: block` for `.area-box` and `.coordinates-display`

