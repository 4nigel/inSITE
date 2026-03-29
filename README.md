# inSITE

**Site complexity analyser for NZ residential construction**

inSITE is a single-file browser tool for analysing building site terrain complexity. Load an IFC terrain mesh, set your site parameters, and get an instant complexity score — with slope heatmaps, setback zones, flood level checks, solar shadow analysis, and 3D form model overlays.

Built for Aotearoa New Zealand residential construction, with NZS 3604 scope limits and council compliance rules built in.

---

## How to use

1. Download `inSITE.html`
2. Open it in any modern browser (Chrome, Edge, Firefox, Safari)
3. Drop your IFC terrain file onto the viewport
4. Adjust parameters in the left panel
5. Click **▶ Run analysis**

**No install. No server. No internet required. Works offline.**

---

## What it analyses

- **Slope** — heatmap per terrain triangle, NZS 3604 scope threshold flagging
- **Elevation** — colour-banded terrain surface
- **Aspect** — compass orientation of each slope face
- **Setbacks** — inward-offset zones from cadastral boundary (front/side/rear)
- **MFL** — Minimum Floor Level compliance against council flood data
- **Shadow** — solar shadow outline and fill draped on terrain surface (SPA algorithm)
- **Form models** — 3D massing overlays (IFC, OBJ, STL) with ground intersection

Outputs a **Site Complexity Index (0–100%)** and an **Impacts on Feasibility** table.

---

## IFC requirements

### Terrain mesh

```
IfcGeographicElement
  Name            = anything
  PredefinedType  = .TERRAIN.
  Geometry        = IfcPolygonalfaceSet or IfcTriangulatedFaceSet
```

Export from Bonsai BIM using the standard terrain mesh export. IFC4X3 (IFC4X3_ADD2) required.

### Title boundary

```
IfcGeographicElement
  Name            = anything
  ObjectType      = 'TitleBoundary'   ← exact string, case-insensitive match
  PredefinedType  = .USERDEFINED.
  Geometry        = IfcIndexedPolyCurve or IfcPolygonalfaceSet
```

Draw the cadastral boundary as a closed polyline in Bonsai, assign as `IfcGeographicElement` with `ObjectType = 'TitleBoundary'`.

### Road frontage (optional)

```
IfcGeographicElement
  ObjectType      = 'RoadFrontage'
  Geometry        = 2-point line snapped to the street-facing boundary edge
```

Without this, use the **↖ Click street boundary edge** fallback in the left panel.

### Form model (optional)

Any `IfcPolygonalfaceSet` geometry in a separate IFC file, OBJ, or STL.  
**Convention:** model Z=0 = finished floor level. Use Offset Z in the left panel to position on site.

---

## FOSS requirements

inSITE is built to strict free and open-source principles. These are non-negotiable for this project.

### Core rule: no closed-source dependencies

Every library, algorithm, and data source must be:

| Requirement | Detail |
|---|---|
| **Open source** | MIT, Apache 2.0, BSD, ISC, or public domain licence |
| **Browser-native where possible** | Canvas 2D API, vanilla JS — no build tools required |
| **No proprietary APIs** | No paid services, no API keys, no cloud dependencies |
| **No telemetry** | No analytics, no tracking, no external requests |
| **Offline capable** | Must function with no internet connection |
| **Single-file deliverable** | All code in one `.html` file — no npm, no bundler, no server |

### Why this matters

inSITE is intended for use in locked-down government and local government environments (NZ public sector, Kāinga Ora programme teams, councils). These environments often block external CDNs, prohibit cloud tools, and require air-gap capability.

### Current dependencies

| Component | Licence | Notes |
|---|---|---|
| Canvas 2D API | Browser-native | Zero external |
| SPA sun position algorithm | Public domain (NREL) | Implemented inline |
| IFC parser | Custom, written for this project | No web-ifc or IfcOpenShell required |
| OBJ / STL parsers | Custom, written for this project | Pure JS, ~60 lines each |
| DM Sans font | [Open Font Licence](https://fonts.google.com/specimen/DM+Sans) | Loaded from Google Fonts — can be self-hosted for full offline use |

### Contributions

All contributions must comply with the FOSS requirements above. Pull requests that introduce closed-source, proprietary, or API-key-dependent code will not be accepted.

---

## Bonsai BIM workflow

1. Import terrain survey (DEM, point cloud, or contour data) into Blender using BlenderGIS
2. Generate TIN mesh and assign as `IfcGeographicElement` with `PredefinedType=TERRAIN` in Bonsai
3. Draw cadastral boundary polyline, assign `ObjectType='TitleBoundary'`
4. Export IFC4X3 from Bonsai
5. Drop into inSITE

---

## Test files

| File | Description |
|---|---|
| `inSITE-TestTIN-Ifc43-v1.ifc` | Small test TIN with title boundary (`IfcPolygonalfaceSet` format) |
| `Stanmore2.ifc` | Real site TIN with title boundary (`IfcIndexedPolyCurve` format) |

---

## Roadmap

- [ ] Perspective toggle for orbit view
- [ ] Draggable panel splitters
- [ ] SVG + PDF report export (Dasu.print integration)
- [ ] District Plan GeoJSON overlay import
- [ ] Services connection point mapping
- [ ] Site comparison mode (multiple IFC files, same ruleset)
- [ ] Recession plane analysis (per boundary edge)
- [ ] `IfcArcIndex` curved boundary support

---

## Related projects

- [Dasu.print](https://github.com/4nigel/dasu.print) — single-file drawing sheet layout tool for Bonsai / OSArch
- [IfcSieve](https://github.com/4nigel/IfcSieve) — browser-based IFC geometry stripper and data extractor
- [Bonsai BIM](https://bonsaibim.org) — open BIM authoring in Blender

---

## Licence

MIT

---

*Built by Nigel Lamb · Christchurch, Aotearoa New Zealand*  
*Part of a FOSS AEC toolset alongside Dasu.print, IfcSieve, RainIn, and TAKT-it*
