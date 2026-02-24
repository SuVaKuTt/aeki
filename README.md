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
Interactive map view that displays areas as rectangular boxes on a floor plan image. Users can select multiple areas simultaneously to visualize their locations on the map. Supports two floor plans: `map_1floor.jpg` (first floor) and `map_2floor.jpg` (second floor). The map automatically switches when selecting areas from different floors.

**Features:**
- Sidebar with two sections:
  - **Section 1: Areas** - Lists all available areas (Area1, Area2, Area3, etc.) with coordinate keys
  - **Section 2: ColorDesc** - Lists all unique color descriptions for filtering
- Area selection with multi-select capability (toggle on/off)
- ColorDesc filtering - select a colorDesc to show all areas containing items with that color description
- Visual representation of areas as rectangular boxes on the map
- Area boxes positioned using coordinates from `coordinates.json`
- Automatic mapping between area names (Area1, Area2, etc.) and coordinate keys (ABCD, EFGHi, etc.)
- Sidebar displays area names with coordinate keys: "Area1 (ABCD)" format
- **Multi-floor support**: Automatically switches between `map_1floor.jpg` and `map_2floor.jpg` based on selected areas
  - Area9, Area10, Area11 are on floor1 (`map_1floor.jpg`)
  - Other areas are on floor2 (`map_2floor.jpg`)
  - When selecting an area from a different floor, all areas from the other floor are automatically deselected
- **Display areas**: Special visual markers (Displays1, Displays2) that appear in the menu automatically
  - Displays1 is on floor2 (`map_2floor.jpg`) - appears in menu between Area8 and Area9
  - Displays2 is on floor1 (`map_1floor.jpg`) - appears in menu after Area11
  - These areas are **visual markers only** - they are not in `export.json` and have no data
  - When selected, they show on the map but Area Info displays "Display area - no data"
  - They are automatically added to the menu if present in `coordinates.json`
- **Info Area** - Right-side panel showing detailed information about active areas:
  - Displays key ranges in format "A1-23", "B4-19", "C5-17" (first number - count)
  - Handles continuous ranges and gaps (e.g., "C5-14" and "C16-17" shown separately)
  - Supports single-letter keys (A1, B2) and multi-letter keys (AB1, AC22)
  - Supports both uppercase and lowercase letters (e.g., "i1" with lowercase "i")
  - Shows total count of keys per area
  - In Area mode: displays all keys for selected areas, plus colorDesc summary at the end
  - In ColorDesc mode: filters keys by selected colorDesc, shows only matching keys
  - Header updates dynamically: "Area Info" (Area mode) or "Area Info, Lighting" (ColorDesc mode)
- Responsive design - boxes scale with image resize
- Support for area labels (text display in boxes)
- Wide sidebar (250px) to prevent text wrapping
- **Local Edit Mode (localhost / file:// only)**:
  - Toggle `Edit mode` to move and resize visible area boxes directly on the map
  - Add/delete boxes for multi-part areas (e.g., `Displays1`)
  - Save active floor coordinates to `coordinates-1floor-edited.json` / `coordinates-2floor-edited.json`
  - Edit tools are hidden on GitHub Pages (public view remains read-only)

**How it works:**

**GitHub Pages (Production):**
- Automatically loads `export.json` to get area names
- Automatically loads `coordinates.json` to get area coordinates
- No buttons visible - everything loads automatically

**Local Development (Windows PC):**
- Two buttons in the sidebar:
  1. **"Open export.json"** - Loads area names from `export.json` file
  2. **"Open coordinates.json"** - Loads area coordinates from `coordinates.json` file
- Edit tools (local only):
  - **"Edit mode: ON/OFF"** - Enables drag/resize for visible boxes
  - **"Add box to selected area"** - Adds another box to the selected coordinate entry
  - **"Delete selected box"** - Deletes selected box (keeps at least one)
  - **"Save active floor JSON"** - Exports only the active floor coordinates
- Use file dialogs to select the respective JSON files
- Both files need to be loaded for full functionality

**Usage:**
1. Load `export.json` to populate both sections in the sidebar:
   - **Section 1 (Areas)**: Shows Area1, Area2, Area3, etc.
   - **Section 2 (ColorDesc)**: Shows all unique color descriptions from the data
2. Load `coordinates.json` to enable area box visualization on the map
   - After loading, sidebar area names update to show coordinate keys: "Area1 (ABCD)", "Area2 (EFGHi)", etc.
   - The mapping is automatic: first coordinate key → Area1, second → Area2, etc.
   - **Display areas** (Displays1, Displays2) are automatically added to the menu if present in `coordinates.json`
     - These are visual markers only and do not appear in `export.json`
     - They show on the map when selected but have no data in Area Info
3. **Two filtering modes:**
   - **Area mode**: Click area names in Section 1 to toggle their selection (multiple areas can be active simultaneously)
   - **ColorDesc mode**: Click a colorDesc in Section 2 to show all areas containing items with that color description (only one colorDesc can be active at a time)
   - Note: Selecting an area deactivates colorDesc selection, and vice versa
4. **Automatic floor switching**: When selecting an area from a different floor (e.g., selecting Area9 when Area1 is active), the map automatically switches to the correct floor plan and deselects all areas from the other floor
5. Selected areas appear as rectangular boxes on the map image
6. Boxes are positioned using relative coordinates (%), so they scale correctly when the image is resized
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
- Coordinate entries support two formats:
  - **Single-box (legacy/current for most areas):** `x`, `y`, `width`, `height`, `relX`, `relY`, `relWidth`, `relHeight`
  - **Multi-box (used for split areas like `Displays1`):** `boxes: [{ x, y, width, height, relX, relY, relWidth, relHeight, name? }]`
- Each coordinate entry also has a `map` field indicating which floor plan image to use: `"map_1floor.jpg"` or `"map_2floor.jpg"`
- **CRITICAL: The order of keys in `coordinates.json` is extremely important!**
  - The mapping is done by index position: first key → Area1, second key → Area2, etc.
  - **The order in `coordinates.json` must match the alphabetical order of keys in the JSON file**
  - JavaScript `Object.keys()` returns keys in the order they appear in the JSON file
  - If you change the order of keys in `coordinates.json`, the area-to-coordinate mapping will change!
  - Example: If "ABCD" is the first key, it maps to Area1. If you move it to second position, it will map to Area2 instead.
  - **Always maintain the correct order when editing `coordinates.json`**
- **Display areas** (Displays1, Displays2):
  - These are special visual markers that appear in the menu automatically
  - They are **not** in `export.json` - they only exist in `coordinates.json`
  - When present in `coordinates.json`, they are automatically added to the menu at the correct position
  - Displays1 should be positioned after Area8 (9th position in coordinates.json)
  - Displays2 should be positioned after Area11 (13th position in coordinates.json)
  - The menu order follows the `coordinates.json` order, so Display areas appear in the correct positions
  - Display areas may use `boxes[]` for multiple visual regions
  - Each display box may have optional `name` (e.g., `displays_A`, `displays_B`) shown on the map label

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
- Note: this is a separate debug helper from the local `Edit mode` for actual area boxes
