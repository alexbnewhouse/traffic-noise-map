# Colorado Traffic Count Data API Endpoints

## Summary

This document provides working ArcGIS REST API endpoints for traffic count (AADT) data in Colorado, including the Denver metro area.

---

## CDOT Traffic Data (Primary Source - State Coverage)

### Traffic Stations (Point Data)
**Base URL:**
```
https://dtdapps.coloradodot.info/arcgis/rest/services/OTIS/TrafficExplorer/MapServer/0
```

**Service Info:** MapServer Layer 0 - TrafficStations (Point geometry)

### Key Fields Available:
| Field | Type | Description |
|-------|------|-------------|
| `AADT` | Integer | **Average Annual Daily Traffic** - Primary traffic count |
| `AADTYR` | String | Year of AADT measurement |
| `AADTSINGLE` | Integer | AADT for single-unit trucks |
| `AADTCOMB` | Integer | AADT for combination trucks |
| `AADTTRUCKS` | Integer | Total truck AADT |
| `AADTDERIV` | String | AADT derivation method code |
| `COUNTYEAR` | String | Year count was conducted |
| `COUNTSTATIONID` | String | Unique station identifier |
| `COUNTLOCATION` | String | Location description |
| `ROUTE` | String | Route designation (e.g., "001A", "025") |
| `REFPT` | Double | Reference point (mile marker start) |
| `ENDREFPT` | Double | End reference point (mile marker end) |
| `CO_FIPS` | Double | County FIPS code |
| `CITY_FIPS` | Double | City FIPS code |
| `DHV` | Double | Design Hour Volume |
| `VCRATIO` | Double | Volume/Capacity ratio |
| `ROUTECAPAC` | Integer | Route capacity |

### Example Queries:

#### 1. Get All Traffic Stations as GeoJSON
```
https://dtdapps.coloradodot.info/arcgis/rest/services/OTIS/TrafficExplorer/MapServer/0/query?where=1%3D1&outFields=*&f=geojson
```

#### 2. Query Traffic in Denver County (FIPS=31)
```
https://dtdapps.coloradodot.info/arcgis/rest/services/OTIS/TrafficExplorer/MapServer/0/query?where=CO_FIPS%3D31&outFields=AADT,AADTYR,COUNTLOCATION,ROUTE&f=geojson
```

#### 3. Query High-Traffic Locations (AADT > 50,000)
```
https://dtdapps.coloradodot.info/arcgis/rest/services/OTIS/TrafficExplorer/MapServer/0/query?where=AADT%3E50000&outFields=*&f=geojson
```

#### 4. Spatial Query (Denver Metro Bounding Box)
```
https://dtdapps.coloradodot.info/arcgis/rest/services/OTIS/TrafficExplorer/MapServer/0/query?where=1%3D1&geometry=-105.2,39.5,-104.6,40.0&geometryType=esriGeometryEnvelope&inSR=4326&spatialRel=esriSpatialRelIntersects&outFields=*&f=geojson&outSR=4326
```

#### 5. Query by Route Number (Interstate 25)
```
https://dtdapps.coloradodot.info/arcgis/rest/services/OTIS/TrafficExplorer/MapServer/0/query?where=ROUTE%20LIKE%20%27025%25%27&outFields=*&f=geojson
```

---

## CDOT Highways (Line Data)
**Base URL:**
```
https://dtdapps.coloradodot.info/arcgis/rest/services/OTIS/TrafficExplorer/MapServer/1
```

**Note:** This layer provides highway line geometry but limited attribute data. For AADT, use the TrafficStations layer (Layer 0).

---

## DRCOG Traffic Counts (Denver Regional Council of Governments) ⭐ PRIMARY SOURCE

### FeatureServer Endpoint (POINT Geometry with Traffic Volumes)

**Base URL:**
```
https://services2.arcgis.com/lCUrzfRwZYxmwIse/arcgis/rest/services/Traffic_Counts_2024_Merged_11242025/FeatureServer/1
```

**Service Info:**
- **Geometry Type:** `esriGeometryPoint` (POINT)
- **Name:** Traffic_Counts_2024_Merged_11242025
- **Coverage:** DRCOG region (Denver metro + surrounding counties)
- **Time Period:** 2010-2024 traffic counts at each location
- **Public Access:** Yes, no authentication required
- **Supported Formats:** JSON, GeoJSON, PBF

### Key Fields:

| Field | Type | Description |
|-------|------|-------------|
| `Location` | String | Road/intersection description |
| `Max_volume` | Double | Maximum recorded volume at this location |
| `v1_date` through `v20_date` | String | Date of traffic count (up to 20 counts per location) |
| `v1_vol` through `v20_vol` | Double | **Daily traffic volume (AADT)** for each count date |
| `count_` | Double | Number of counts at this location |
| `Jan_2022` - `Dec_2024` | Double | Monthly traffic volumes (recent years) |
| `gid` | String | Unique location identifier |

**Note:** Traffic volume values are stored as **daily totals (both directions)** - equivalent to AADT.

### Example Queries:

#### 1. Get All Traffic Count Locations as GeoJSON
```
https://services2.arcgis.com/lCUrzfRwZYxmwIse/arcgis/rest/services/Traffic_Counts_2024_Merged_11242025/FeatureServer/1/query?where=1%3D1&outFields=*&f=geojson&outSR=4326
```

#### 2. Get Basic Fields (Location + Most Recent Volume)
```
https://services2.arcgis.com/lCUrzfRwZYxmwIse/arcgis/rest/services/Traffic_Counts_2024_Merged_11242025/FeatureServer/1/query?where=1%3D1&outFields=Location,Max_volume,v1_date,v1_vol&f=geojson&outSR=4326
```

#### 3. Query High-Traffic Locations (Max Volume > 50,000)
```
https://services2.arcgis.com/lCUrzfRwZYxmwIse/arcgis/rest/services/Traffic_Counts_2024_Merged_11242025/FeatureServer/1/query?where=Max_volume%3E50000&outFields=Location,Max_volume&f=geojson&outSR=4326
```

#### 4. Spatial Query (Denver Area Bounding Box)
```
https://services2.arcgis.com/lCUrzfRwZYxmwIse/arcgis/rest/services/Traffic_Counts_2024_Merged_11242025/FeatureServer/1/query?where=1%3D1&geometry=-105.1,39.6,-104.8,39.85&geometryType=esriGeometryEnvelope&inSR=4326&spatialRel=esriSpatialRelIntersects&outFields=Location,Max_volume,v1_date,v1_vol&f=geojson&outSR=4326
```

#### 5. Get 2024 Monthly Traffic Data
```
https://services2.arcgis.com/lCUrzfRwZYxmwIse/arcgis/rest/services/Traffic_Counts_2024_Merged_11242025/FeatureServer/1/query?where=1%3D1&outFields=Location,Jan_2024,Feb_2024,Mar_2024,Apr_2024,May_2024,Jun_2024,Jul_2024,Aug_2024,Sep_2024,Oct_2024,Nov_2024,Dec_2024&f=geojson&outSR=4326
```

### Python Example: Fetching DRCOG Traffic Data

```python
import requests

def get_drcog_traffic_data(bbox=None, max_results=2000):
    """Fetch DRCOG traffic count data for Denver metro area."""
    
    base_url = "https://services2.arcgis.com/lCUrzfRwZYxmwIse/arcgis/rest/services/Traffic_Counts_2024_Merged_11242025/FeatureServer/1/query"
    
    params = {
        'where': '1=1',
        'outFields': 'Location,Max_volume,v1_date,v1_vol,v2_date,v2_vol,count_',
        'f': 'geojson',
        'outSR': '4326',
        'resultRecordCount': max_results
    }
    
    # Add spatial filter if bbox provided
    if bbox:
        params.update({
            'geometry': ','.join(map(str, bbox)),  # [xmin, ymin, xmax, ymax]
            'geometryType': 'esriGeometryEnvelope',
            'inSR': '4326',
            'spatialRel': 'esriSpatialRelIntersects'
        })
    
    response = requests.get(base_url, params=params)
    return response.json()

# Usage - Get all data
data = get_drcog_traffic_data()
print(f"Found {len(data['features'])} traffic count locations")

# Usage - Get data for downtown Denver area
denver_bbox = [-105.02, 39.73, -104.95, 39.77]
denver_data = get_drcog_traffic_data(bbox=denver_bbox)

# Example: Find highest traffic locations
for feature in sorted(data['features'], 
                      key=lambda x: x['properties'].get('Max_volume') or 0, 
                      reverse=True)[:10]:
    props = feature['properties']
    print(f"Location: {props['Location']}")
    print(f"Max Volume: {props['Max_volume']:,.0f}")
    print(f"Most Recent Count: {props.get('v1_date')} - {props.get('v1_vol'):,.0f}")
    print("---")
```

### JavaScript Example: Fetching DRCOG Traffic Data

```javascript
async function getDRCOGTrafficData(options = {}) {
    const baseUrl = 'https://services2.arcgis.com/lCUrzfRwZYxmwIse/arcgis/rest/services/Traffic_Counts_2024_Merged_11242025/FeatureServer/1/query';
    
    const params = new URLSearchParams({
        where: '1=1',
        outFields: 'Location,Max_volume,v1_date,v1_vol',
        f: 'geojson',
        outSR: '4326'
    });
    
    // Add bbox filter if provided
    if (options.bbox) {
        params.set('geometry', options.bbox.join(','));
        params.set('geometryType', 'esriGeometryEnvelope');
        params.set('inSR', '4326');
        params.set('spatialRel', 'esriSpatialRelIntersects');
    }
    
    const response = await fetch(`${baseUrl}?${params}`);
    return response.json();
}

// Usage
getDRCOGTrafficData().then(data => {
    console.log(`Found ${data.features.length} traffic count locations`);
    
    // Get coordinates and traffic volume for each location
    data.features.forEach(f => {
        const [lng, lat] = f.geometry.coordinates;
        const volume = f.properties.Max_volume;
        console.log(`${f.properties.Location}: ${volume} at [${lat}, ${lng}]`);
    });
});
```

### Story Map Source
This endpoint powers the DRCOG Regional Traffic Counts story map:
- **Story Map:** https://experience.arcgis.com/experience/d31c861edab6491ab20909e65a794710

### Additional DRCOG Resources
- **DRCOG Data Catalog:** https://data.drcog.org/dataset/traffic-counts
- **DRCOG ArcGIS Portal:** https://DRCOG.maps.arcgis.com

---

## Query Parameters Reference

### Common Parameters:
| Parameter | Description | Example |
|-----------|-------------|---------|
| `where` | SQL-like filter | `AADT>10000` |
| `outFields` | Fields to return | `AADT,ROUTE,COUNTLOCATION` or `*` for all |
| `f` | Output format | `geojson`, `json`, `pjson` |
| `geometry` | Spatial filter (bbox) | `-105.2,39.5,-104.6,40.0` |
| `geometryType` | Type of geometry filter | `esriGeometryEnvelope`, `esriGeometryPoint` |
| `spatialRel` | Spatial relationship | `esriSpatialRelIntersects` |
| `inSR` | Input spatial reference | `4326` (WGS84) |
| `outSR` | Output spatial reference | `4326` (WGS84) |
| `resultRecordCount` | Limit results | `1000` |
| `resultOffset` | Pagination offset | `0`, `1000`, etc. |

### Supported Output Formats:
- `json` - Esri JSON format
- `geojson` - Standard GeoJSON
- `pjson` - Pretty-printed JSON
- `pbf` - Protocol Buffers

---

## Python Example: Fetching Denver Traffic Data

```python
import requests

def get_denver_traffic_data():
    """Fetch traffic count data for Denver metro area."""
    
    base_url = "https://dtdapps.coloradodot.info/arcgis/rest/services/OTIS/TrafficExplorer/MapServer/0/query"
    
    # Denver metro bounding box (approximate)
    params = {
        'where': '1=1',
        'geometry': '-105.3,39.5,-104.5,40.1',  # Denver metro bbox
        'geometryType': 'esriGeometryEnvelope',
        'inSR': '4326',
        'spatialRel': 'esriSpatialRelIntersects',
        'outFields': 'AADT,AADTYR,AADTTRUCKS,COUNTLOCATION,ROUTE,REFPT',
        'f': 'geojson',
        'outSR': '4326'
    }
    
    response = requests.get(base_url, params=params)
    return response.json()

# Usage
data = get_denver_traffic_data()
print(f"Found {len(data['features'])} traffic count stations")

# Example: Filter high-traffic locations
for feature in data['features']:
    aadt = feature['properties'].get('AADT', 0)
    if aadt and aadt > 100000:
        print(f"Location: {feature['properties']['COUNTLOCATION']}")
        print(f"AADT: {aadt:,}")
        print(f"Route: {feature['properties']['ROUTE']}")
        print("---")
```

---

## JavaScript/Fetch Example

```javascript
async function getDenverTrafficData() {
    const baseUrl = 'https://dtdapps.coloradodot.info/arcgis/rest/services/OTIS/TrafficExplorer/MapServer/0/query';
    
    const params = new URLSearchParams({
        where: '1=1',
        geometry: '-105.3,39.5,-104.5,40.1',
        geometryType: 'esriGeometryEnvelope',
        inSR: '4326',
        spatialRel: 'esriSpatialRelIntersects',
        outFields: 'AADT,AADTYR,COUNTLOCATION,ROUTE',
        f: 'geojson',
        outSR: '4326'
    });
    
    const response = await fetch(`${baseUrl}?${params}`);
    const data = await response.json();
    return data;
}

// Usage
getDenverTrafficData().then(data => {
    console.log(`Found ${data.features.length} stations`);
});
```

---

## Authentication

**No authentication required** - These are public endpoints provided by CDOT.

---

## Rate Limits & Best Practices

1. **Pagination**: Default max records per request is 1000. Use `resultRecordCount` and `resultOffset` for pagination.
2. **Spatial Filtering**: Use bounding boxes to limit results to your area of interest.
3. **Field Selection**: Specify only needed fields in `outFields` to reduce response size.
4. **Caching**: Data is typically updated annually; cache responses appropriately.

---

## County FIPS Codes (Denver Metro)

| County | FIPS Code |
|--------|-----------|
| Denver | 31 |
| Jefferson | 59 |
| Adams | 1 |
| Arapahoe | 5 |
| Douglas | 35 |
| Boulder | 13 |
| Broomfield | 14 |

### Query Example for Denver Metro Counties:
```
https://dtdapps.coloradodot.info/arcgis/rest/services/OTIS/TrafficExplorer/MapServer/0/query?where=CO_FIPS%20IN%20(31,59,1,5,35,13,14)&outFields=*&f=geojson
```

---

## Additional Resources

- **CDOT Traffic Data Explorer**: https://dtdapps.coloradodot.info/otis/trafficdata
- **CDOT Traffic Volume Web Map**: https://experience.arcgis.com/experience/ab7c09a831be45148991181947a97e12
- **CDOT Open Data**: https://data-cdot.opendata.arcgis.com/
- **DRCOG Data Catalog**: https://data.drcog.org/

---

## Notes

1. **Data Currency**: AADT values are typically from the most recent traffic counts (often 1-2 years old).
2. **Coverage**: CDOT data covers state highways. Local roads may not have AADT data.
3. **Coordinate System**: Default is Web Mercator (EPSG:3857). Request WGS84 (EPSG:4326) using `outSR=4326`.
