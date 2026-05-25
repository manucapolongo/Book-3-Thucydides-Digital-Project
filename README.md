# Book-3-Thucydides-Digital-Project
The folder Data contains the datasets produces, the folder maps the maps in Qgis.

## Data Files

### `annotations.csv`
Raw export from Recogito. Contains every person or collective annotated in Book 3, with original Recogito columns: `UUID`, `FILE`, `QUOTE_TRANSCRIPTION`, `ANCHOR`, `TYPE`, `TAGS`. Tags are pipe-separated key-value pairs encoding origin, Pleiades ID, side, and anonymity status.

### `annotations_all_mentions.csv`
Same 651 rows as the raw export, enriched with parsed tag columns (`origin`, `pleiades_id`, `side`, `anonymous`) and Pleiades coordinates (`latitude`, `longitude`)

### `annotations_by_origin.csv`
One row per unique place of origin (50 rows). Contains aggregated counts: `person_count` (total persons from that place), `individual_count` (named individuals), `group_count` (anonymous collectives), `pct_named` (percentage of named individuals), coordinates, and a semicolon-separated list of all persons. 

### `annotations_side_athenian.csv`
All 347 annotated persons and collectives fighting on the Athenian side, with full enriched columns.

### `annotations_side_spartan.csv`
All 303 annotated persons and collectives fighting on the Spartan side, with full enriched columns. 

### `11_annotations_repeated_individuals.csv`
The 30 named individuals who appear more than once in the text (151 rows total), with an added `mention_count` column. 

### `booke3_places.csv`
Cleaned and deduplicated geographical places mentioned in Book 3 (51 unique places). Duplicate Pleiades IDs for the same location have been consolidated; different names of places have been merged into a single entry. Contains `mention_count` (unique citations), `person_count` and `individual_count` from the prosopographical dataset where available, and`pct_named'.

### `book3_places_all_mentions_cleaned.csv`
All 195 individual geographical place-mentions from the Perseus Pleiades dataset for Book 3, after cleaning and deduplication by `(pleiades_place_id, citation_reference)`. Each row is a unique mention of a place in a specific passage. Contains the original Perseus annotation fields plus enriched `place_name`, `latitude`, and `longitude`.

---

## Maps 

All maps use the AWMC tile layer as base map and EPSG:4326 (WGS 84) as the coordinate reference system. Open with QGIS 3.x or later.
All CSV files must be in the same folder as the `.qgz` file for the layers to load correctly.

### `map_1.qgz` — Bubble map: persons by place of origin
Displays all unique places of origin as circles coloured by `pct_named` using a graduated colour scale from white (0%) to red (100%). Shows the rate of individualisation — the proportion of references that are to named individuals rather than anonymous collectives — across all places of origin. The size of each circle is proportional to the total number of references (`person_count`) associated with that place.

**Layers:**
- `annotations_by_origin.csv` — point layer, X=`longitude`, Y=`latitude`, graduated symbol by `person_count`
- AWMC tiles — base map


### `map_2.qgz` — Choropleth: percentage of named individuals
Displays the same data as map 1 but as pie charts. Each chart is divided into two segments: blue for `individual_count` (named persons) and red for `group_count` (anonymous collectives). The size of each pie chart is proportional to the total number of references (`person_count`) associated with that place.

**Layers:**
- `annotations_by_origin.csv` — point layer, X=`longitude`, Y=`latitude`, graduated colour by `pct_named`
- AWMC tiles — base map

### `map_3.qgz` — Individualisation rate with political side overlay
Displays `pct_named` on the same white-to-red colour scale as map 1, but adds a political allegiance overlay: cities on the Spartan side are marked with a purple outline, cities on the Athenian side with a yellow outline.

**Layers:**
- `annotations_by_origin.csv` — base colour layer, graduated by `pct_named`
- `annotations_side_athenian.csv` — yellow outline overlay, Athenian-side origins, 347 rows
- `annotations_side_spartan.csv` — purple outline overlay, Spartan-side origins, 303 rows
- AWMC tiles — base map

### `map_4.qgz` — Geographical place-mentions vs individualisation ratio
Displays the 51 cleaned geographical locations mentioned in Book 3. Circle size represents `mention_count` (the total number of geographical references to that place); colour represents the ratio between references to named individuals and total geographical mentions. This ratio is calculated differently from `pct_named` in maps 1–3 and the two figures are not directly comparable.

**Layers:**
- `booke3_places.csv` — point layer, X=`longitude`, Y=`latitude`, graduated symbol by `mention_count`.
- AWMC tiles — base map

### `map_5.qgz` — Repeat individuals (mentioned at least twice)
Dysplays the number of individuals mentioned more than twice coming from a place.

**Layers:**
- `11_annotations_repeated_individuals.csv` — point layer, X=`longitude`, Y=`latitude`, graduated symbol by `mention_count`, labelled by `QUOTE_TRANSCRIPTION`
- AWMC tiles — base map

## Licence
All original datasets and map files in this repository are released under Creative Commons Zero v1.0 Universal (CC0) — no rights reserved. You may copy, modify, and distribute this data without asking permission or providing attribution.

## Attribution and Data Sources
This project incorporates data from several external sources, each with its own licence. When reusing the datasets in this repository, please attribute the following sources separately:

### Pleiades Gazetteer (geographical coordinates for all CSV files)

Elliott, Tom, Roger Bagnall, Richard Talbert, et al. (eds.). Pleiades: A Gazetteer of Past Places. Institute for the Study of the Ancient World, NYU, 2006–present. https://pleiades.stoa.org. Licensed under Creative Commons Attribution 3.0 (CC BY 3.0). Attribution is required when reusing coordinate data derived from Pleiades.

### Perseus Digital Library (place-mention annotation dataset for Thucydides)

Crane, Gregory (ed.). Perseus Digital Library. Tufts University, 1987–present. https://www.perseus.tufts.edu. The dataset of place annotations for Thucydides' History of the Peloponnesian War was sourced from Perseus and filtered to Book 3 for this project.

### Thucydides, History of the Peloponnesian War (base text)

Thucydides. History of the Peloponnesian War. Translated by J. M. Dent and E. P. Dutton (Crawley translation), 1914. Available via the Perseus Digital Library. Public domain.
