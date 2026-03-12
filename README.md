# Family Tree - Radial Genealogy Viewer

**[Live Demo](https://mancmit.github.io/family-tree)**

A single-file HTML application that displays family trees in a radial (concentric circle) layout with a warm vintage theme.

## Features

- **Radial Layout** — Members are arranged in concentric rings by generation, using a polar-to-Cartesian coordinate system
- **CRUD Operations** — Add, edit, and delete family members via modal forms
- **Detail Panel** — Click any node to view full info, relationships, and actions
- **Branch View** — Focus on a specific person's branch (1 generation up, 2 down)
- **Kinship Terms** — Select two people to see their Vietnamese kinship terms (Cha/Mẹ, Ông/Bà, Chú/Bác/Cô/Cậu/Dì, etc.)
- **Zoom & Pan** — Mouse drag, scroll wheel, and touch pinch support (0.2x–3.0x)
- **Search** — Filter members by name, click to pan to their node
- **Import/Export** — Save and load family data as JSON files
- **Sample Data** — Includes a 30-person, 5-generation sample family

## Getting Started

Open `index.html` in any modern browser, or visit the [Live Demo](https://mancmit.github.io/family-tree). No build tools or server required.

On the welcome screen you can:
1. Enter a family name and start a new tree
2. Load the built-in sample data
3. Import from a JSON file

## Theme

**Warm Vintage** — parchment/brown tones with gold accents.

All colors and sizing are managed through CSS custom properties (`:root` variables). Five generation colors distinguish depth: Gold, Olive, Burnt Sienna, Dusty Blue, and Rose. Gender is indicated with blue (male) and rose (female).

## Visual Effects

- Floating gold particles (CSS animation)
- Paper grain texture (SVG noise + dot pattern overlay)
- Vignette edge gradient
- Animated connection lines (flowing dashes, pulsing dots, heartbeat on spouse links)
- Glassmorphism node cards with letter avatars and generation badges

## Data Model

```json
{
  "familyName": "string",
  "people": [
    {
      "id": 1,
      "name": "string",
      "gender": "male | female",
      "birthYear": "string",
      "deathYear": "string",
      "birthplace": "string",
      "note": "string",
      "parentId": null,
      "spouseId": null
    }
  ],
  "nextId": 2
}
```

- **Parent-child**: `parentId` on the child points to one parent
- **Spouse**: `spouseId` is a bidirectional link between two people
- **Persistence**: `localStorage` (key: `familyTreeState`)

## Layout Algorithm

1. `findRoots()` — Identify root nodes (no parent, not spouse-only)
2. `minSweep()` — Recursively compute the minimum angle each subtree requires
3. `assignAngles()` — Distribute angles proportionally across subtrees
4. Convert polar `(angle, ring)` to Cartesian `(x, y)` pixel positions

Spouses are offset to either side of the midpoint angle. Layout parameters (`--ring-gap`, `--ring-min-radius`, `--node-w`, `--node-h`) are configurable via CSS variables.

## Kinship Terms (Xưng Hô)

Select a person, click "Xưng hô", then click a second person to see how they address each other in Vietnamese. Supports:

- Direct lineage: Cha/Mẹ, Ông/Bà (nội/ngoại), Cụ, Kỵ
- Siblings: Anh/Chị/Em (by age)
- Uncle/Aunt: Bác/Chú/Cô (paternal), Cậu/Dì (maternal)
- Cousins: Anh/Chị/Em họ
- Spouse: Vợ/Chồng
- In-law: Con dâu/rể, Cha/Mẹ chồng/vợ, etc.

## Browser Support

Works in all modern browsers (Chrome, Firefox, Safari, Edge). No external dependencies — fonts are loaded from Google Fonts.
