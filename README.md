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
- Sidebar with two sections:
  - **Section 1: Areas** - Lists all available areas (Area1, Area2, Area3, etc.) with coordinate keys
  - **Section 2: ColorDesc** - Lists all unique color descriptions for filtering
- Area selection with multi-select capability (toggle on/off)
- ColorDesc filtering - select a colorDesc to show all areas containing items with that color description
- Visual representation of areas as rectangular boxes on the map
- Area boxes positioned using coordinates from `coordinates.json`
- Automatic mapping between area names (Area1-7) and coordinate keys (ABCD, EFGHi, etc.)
- Sidebar displays area names with coordinate keys: "Area1 (ABCD)" format
- **Info Area** - Right-side panel showing detailed information about active areas:
  - Displays key ranges in format "A1-23", "B4-19", "C5-17" (first number - count)
  - Handles continuous ranges and gaps (e.g., "C5-14" and "C16-17" shown separately)
  - Supports single-letter keys (A1, B2) and multi-letter keys (AB1, AC22)
  - Shows total count of keys per area
  - In Area mode: displays all keys for selected areas, plus colorDesc summary at the end
  - In ColorDesc mode: filters keys by selected colorDesc, shows only matching keys
  - Header updates dynamically: "Area Info" (Area mode) or "Area Info, Lighting" (ColorDesc mode)
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
1. Load `export.json` to populate both sections in the sidebar:
   - **Section 1 (Areas)**: Shows Area1, Area2, Area3, etc.
   - **Section 2 (ColorDesc)**: Shows all unique color descriptions from the data
2. Load `coordinates.json` to enable area box visualization on the map
   - After loading, sidebar area names update to show coordinate keys: "Area1 (ABCD)", "Area2 (EFGHi)", etc.
   - The mapping is automatic: first coordinate key → Area1, second → Area2, etc.
3. **Two filtering modes:**
   - **Area mode**: Click area names in Section 1 to toggle their selection (multiple areas can be active simultaneously)
   - **ColorDesc mode**: Click a colorDesc in Section 2 to show all areas containing items with that color description (only one colorDesc can be active at a time)
   - Note: Selecting an area deactivates colorDesc selection, and vice versa
4. Selected areas appear as rectangular boxes on the map image
5. Boxes are positioned using relative coordinates (%), so they scale correctly when the image is resized
6. **Info Area** (right side panel) displays detailed information:
   - **Area mode**: Shows all keys for selected areas in format "A1-23" (letter + first number - count)
     - Handles gaps in numbering (e.g., "C5-14" and "C16-17" shown as separate lines)
     - Supports multi-letter keys (AB1, AC22, etc.)
     - Shows total count per area: "Area1 (ABCD) (52 pcs)"
     - Displays colorDesc summary at the end: "Items: Storage, Bathroom, Lighting"
   - **ColorDesc mode**: Shows only keys matching the selected colorDesc
     - Header shows selected colorDesc: "Area Info, Lighting"
     - Filters keys to show only those with matching colorDesc value
     - Example: If "Storage" is selected, only shows keys where colorDesc = "Storage"

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

