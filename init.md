# Traffic Data Visualization Web App - Initialization Document

## Project Overview
Build a simple, easy-to-install web application that helps users visualize traffic data for house hunting in Arvada, CO (and surrounding Denver metro area). The app should allow users to pin addresses from Zillow and overlay them on traffic volume data from government sources.

## Core Requirements

### Primary Features
1. **Interactive Map Interface**
   - Display a map centered on Arvada, CO (39.8028° N, 105.0875° W)
   - Allow panning and zooming across the Denver metro area
   - Show traffic volume data as color-coded overlays on roads

2. **Address Pinning**
   - Allow users to manually enter addresses or coordinates
   - Enable copy-paste of addresses from Zillow listings
   - Display pins/markers for saved addresses on the map
   - Store pinned addresses locally (localStorage or similar)
   - Allow users to label pins (e.g., "123 Main St - 3BR house")
   - Enable deletion of pins

3. **Traffic Data Visualization**
   - Display traffic count data from available sources (see below)
   - Color-code roads by traffic volume (AADT - Average Annual Daily Traffic)
   - Show tooltip/popup with traffic details when clicking road segments
   - Include legend explaining color coding and AADT values

### Technical Requirements
- **Stack**: Single HTML file with embedded CSS/JavaScript (or minimal separate files)
- **Installation**: Should work by simply opening the HTML file in a browser (no build process)
- **Dependencies**: Use CDN-hosted libraries only (no npm install required)
- **Storage**: Use browser localStorage for saving pinned addresses
- **Responsive**: Should work on desktop browsers (mobile optional)

## Data Sources

### Primary Sources (Denver Metro Area)
1. **DRCOG Regional Traffic Count Program**
   - URL: https://www.drcog.org/transportation-planning/transportation-analysis/regional-traffic-count-program
   - Story Map: https://experience.arcgis.com/experience/d31c861edab6491ab20909e65a794710
   - Data Catalog: https://data.drcog.org/dataset/traffic-counts
   - Coverage: Denver metro area including Arvada
   - Data: Traffic counts since 2010, AADT values

2. **Jefferson County Traffic Data**
   - URL: https://www.jeffco.us/4045/Traffic-Counts
   - Coverage: Jefferson County (Arvada is in Jefferson County)
   - Note: May require accessing their jMap or ArcGIS Hub

3. **Colorado DOT (CDOT) Traffic Volume Map**
   - URL: https://dtdapps.coloradodot.info/otis/trafficdata
   - Main portal: https://dtdapps.coloradotinfo/otis
   - ArcGIS Experience: https://experience.arcgis.com/experience/ab7c09a831be45148991181947a97e12
   - Coverage: Statewide, including state highways

## Implementation Approach

### Phase 1: Basic Map (Start Here)
1. Set up a basic HTML page with a map library (recommend Leaflet.js via CDN)
2. Center map on Arvada, CO
3. Add OpenStreetMap tiles as base layer
4. Implement address input field with geocoding (use free API like Nominatim)
5. Add ability to place and save markers with labels

### Phase 2: Traffic Data Integration
1. Research the data source APIs/formats:
   - Check if DRCOG data catalog has downloadable GeoJSON/CSV
   - Investigate if ArcGIS Experience maps expose public REST APIs
   - Determine if data needs to be manually downloaded or can be fetched dynamically
2. Load traffic count data onto the map
3. Style road segments by AADT values with color gradient
4. Add interactive tooltips showing traffic details

### Phase 3: Polish
1. Add legend for traffic volume colors
2. Implement data filters (e.g., show only roads above certain AADT)
3. Add export/import functionality for pinned addresses
4. Improve UI/UX with better styling

## Suggested Technology Stack

### Mapping Library
- **Leaflet.js** (recommended) - Simple, lightweight, well-documented
- CDN: `https://unpkg.com/leaflet@1.9.4/dist/leaflet.js`

### Geocoding
- **Nominatim** (OpenStreetMap) - Free, no API key required
- API: `https://nominatim.openstreetmap.org/search`
- Alternative: Mapbox Geocoding (requires free API key)

### UI Framework (Optional)
- Plain HTML/CSS/JavaScript is fine
- Or use a simple CSS framework like Bulma or Bootstrap via CDN

### Data Format
- Prefer GeoJSON for geographic data
- CSV with lat/lon coordinates as fallback

## User Workflow

1. User opens the app (HTML file) in browser
2. Map displays centered on Arvada with base layer
3. Traffic volume data is visible as colored road overlays
4. User pastes address from Zillow into input field
5. App geocodes address and places marker on map
6. User adds a label (e.g., "Option 1 - 3BR ranch")
7. User can see traffic volumes on nearby roads by clicking them
8. User saves progress (pins persist in localStorage)
9. User compares multiple properties by their proximity to high-traffic roads

## Success Criteria

### Must Have
- ✅ Map displays and is interactive
- ✅ Users can add address pins manually
- ✅ Pins persist between sessions
- ✅ Some traffic data is visible on the map
- ✅ App works by opening HTML file (no server required)

### Nice to Have
- Multiple traffic data sources integrated
- Time-based traffic patterns (if available in data)
- Export pins as JSON or CSV
- Print-friendly view
- Satellite/aerial imagery base layer option

## Important Notes

### Data Access Considerations
- Many government GIS systems use ArcGIS REST services
- These often have public endpoints that don't require authentication
- Example endpoint pattern: `https://services.arcgis.com/[id]/arcgis/rest/services/[name]/FeatureServer/0/query`
- May need to reverse-engineer the story map URLs to find underlying data services

### Performance
- Large traffic datasets may need to be filtered/simplified
- Consider loading data only for visible map bounds
- Use clustering for address pins if many are added

### Geocoding Rate Limits
- Nominatim has usage limits (1 request/second)
- Implement debouncing or add delay between requests
- Cache geocoding results in localStorage

### Browser Compatibility
- Target modern browsers (Chrome, Firefox, Safari, Edge)
- Use standard JavaScript (ES6+ is fine)

## File Structure (Suggested)

```
traffic-map-app/
├── index.html          (Main application file)
├── styles.css          (Optional: separate CSS)
├── app.js             (Optional: separate JavaScript)
└── README.md          (Usage instructions)
```

Or everything in a single `index.html` for maximum simplicity.

## Getting Started

1. First, investigate the data sources and determine the best way to access traffic data programmatically
2. Create a proof-of-concept with just the map and address pinning
3. Add traffic data visualization once data access is confirmed
4. Iterate on UI and features

## Questions to Resolve During Development

1. What format is the DRCOG traffic data in? (GeoJSON, Shapefile, CSV?)
2. Can we access it via direct API or does it need to be downloaded?
3. What's the data refresh frequency? (Are static files acceptable?)
4. Should we show traffic counts as points or style the actual road geometries?
5. Do we need historical data or just current/recent counts?

## Example Traffic Volume Color Scale

```
AADT Value Range    | Color      | Description
--------------------|------------|------------------
0 - 1,000          | Green      | Very low traffic
1,000 - 5,000      | Yellow     | Low traffic
5,000 - 15,000     | Orange     | Moderate traffic
15,000 - 30,000    | Red        | High traffic
30,000+            | Dark Red   | Very high traffic
```

Adjust based on actual data distribution in Arvada area.

## Additional Context

This tool is for house hunting purposes. High traffic roads generally mean:
- More consistent noise throughout the day
- Higher truck traffic percentage = more noise
- Roads with 20,000+ AADT are notably loud
- Consider both proximity and AADT when evaluating properties

Good luck! Focus on getting a working prototype quickly, then iterate on data integration and features.