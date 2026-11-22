Classtrail: data derived from OpenStreetMap
============================================

Contains information from OpenStreetMap, which is made available here under the Open Database
License (ODbL) 1.0: https://opendatacommons.org/licenses/odbl/1-0/
(c) OpenStreetMap contributors, https://www.openstreetmap.org/copyright

Some buildings of the street maps were added from Overture Maps (https://overturemaps.org), a database that is also
under the ODbL; its contributors include Esri Community Maps contributors (CC BY 4.0), Microsoft (ODbL) and Google
(CC BY 4.0).

What this is: the files below are the derived databases that the Classtrail app carries inside it (map shapes,
the 3D map, walking network, bicycle network, entrances, places, addresses). You may copy, distribute and adapt them under the ODbL: credit
OpenStreetMap, say that you changed them if you did, and offer what you publish from them under the same licence.

What this is NOT: the rooms, floor plans and the names taken from KTH Places belong to KTH and are not part of this
offer. In gyms.json, hours.json and scooter_parking.json the field "sources" says where each kind of entry comes from; only
the OpenStreetMap part is offered under the ODbL here, the other sources keep their own terms.

Coordinates are integers (degrees x 1,000,000) in most files. bike_graph.bin is a binary file (the format is the
docstring of bike_graph_codec.py below, which has a reader and a writer), and so are vector_region_buildings.bin,
vector_region_zones.bin and vector_region_paths.bin (coordinates are whole numbers of 1/2^27 and 1/2^26 of the width of the Web Mercator world; the format is the
docstring of region_pack.py below, which has a reader and a writer); map_labels.bin is a binary file too (its format is the docstring of map_labels_pack.py, with a reader and a writer, and the rules of
which object has a label are map_labels_osm.py and build_map_labels.py). The format of each file is described below.
Questions: classtrail.app@gmail.com

FILES
-----
vector_map.json: Street map of the main campus: buildings, roads, water, green
vector_region.json: Coarse street map of greater Stockholm around the main campus: water, green, streets, roads in tunnels and on bridges, railways, waterways, piers, ferry routes and runways
vector_outer.json: Coarser street map of the ring around greater Stockholm, so that every campus has map all around it (as the region file, with every street of the towns of the ring)
vector_flemingsberg.json: Street map of the Flemingsberg campus
vector_ersta.json: Street map of the Ersta campus
vector_frescati.json: Street map of the Frescati campus
vector_haninge.json: Street map of the Haninge campus
vector_kista.json: Street map of the Kista campus
vector_sodertalje.json: Street map of the Sodertalje campus
vector_solna.json: Street map of the Solna campus
walk_graph.json: Walking network of the campuses (nodes and edges)
doors.json: Entrances of the buildings (OpenStreetMap entrance nodes; a guess where there is none)
door_refs.json: House letters of the entrances of the buildings that have several houses (the letter of the house number of an OpenStreetMap entrance node)
campus.json: Places near the campuses: food places, shops (supermarkets, convenience stores, kiosks, greengrocers, bakeries), pharmacies, cash machines, post offices and parcel lockers, drinking water, bike shops, toilets (with whether they are paid and for whom), defibrillators, chargers, and the named buildings of the campuses
addresses.json: Address points (street and house number)
building_labels_osm.json: The names of the buildings that OpenStreetMap gives and the street addresses that label the others (rows of rank 1, 2 and 3 of the file building_labels.json of the app: text, latitude, longitude, rank, area, key, kind). The 50 rows of rank 0, whose names are taken from KTH Places, are NOT in this file and are not offered under this licence
gyms.json: Gyms and outdoor gyms near the campuses (OpenStreetMap places plus facts named in the file)
hours.json: Opening times and web pages of the places of the map: the opening_hours and website tags of OpenStreetMap, and times read on official pages (marked in the file, with their own terms)
scooter_parking.json: Scooter parking places (open data of Stockholm city, OpenStreetMap; sources named in the file)
access.json: Benches, accessible toilets, parking for disabled people, lifts and step-free entrances, and the wheelchair tags of places
institution_places.json: Buildings of institutions that OpenStreetMap draws and does not name (the campus of GIH, two buildings of the School of Economics, three buildings of Karolinska Institutet): the id and the position of the object of OpenStreetMap and the name that the institution writes
bike_parking.json: Places to leave a bike, an e-bike or an e-scooter near the campuses, with what OpenStreetMap says about each one (roof, lockers, how many bikes, a charger for e-bikes, a fee, who may use it)
map3d.pmtiles: The 3D map: ONE archive of vector tiles (PMTiles v3, zoom 6 to 15) of the ground (with the trees and the decks of the bridges: map3d/build_extras.py makes them from the same extract), the buildings with their heights, the ribbons of glass of tall towers and the names of the streets, places, waters, parks, stations and buildings of everything that the street maps cover (the campuses, greater Stockholm and the ring round it)
vector_region_buildings.bin: Buildings of the whole area of the street maps outside the boxes of the campuses (OpenStreetMap buildings, with the courtyards of the big ones; binary, whole numbers of 1/2^27 of the width of the world)
vector_region_zones.bin: Land use of the whole area of the street maps outside the boxes of the campuses: housing, shops and offices, industry, school and hospital grounds, car parks, pitches, pedestrian areas, platforms, sand and rock, wetland, railway land, farmland, allotments, golf courses, quarries and landfill, the airfield, heath and orchards (OpenStreetMap areas; binary, whole numbers of 1/2^26 of the width of the world)
vector_region_paths.bin: Paths of the whole area of the street maps outside the boxes of the campuses: service roads and tracks, footways, paths, cycleways and bridleways, and steps, as open lines with their class and level (on the surface, on a bridge, in a tunnel) (OpenStreetMap highway ways; binary, whole numbers of 1/2^26 of the width of the world)
map_labels.bin: The words of the 2D map: the names of streets (a label every 300 m of each named street), places (suburbs, islands, villages), lakes and bays, parks and nature reserves, stations, shops and services and buildings of the whole area of the street maps, each with its class, the zoom from which it is written, its priority and its place (OpenStreetMap names, in the language of the place; binary, whole numbers of 1/2^23 of the width of the world)
bike_graph.bin: Bicycle network of greater Stockholm, Sodertalje and Haninge: cycle ways, paths, streets and what a rider can do on them, as nodes and edges (binary)

FORMAT NOTES (from the scripts that build the files)
---------------------------------------------------

[build_vector_map.py]
Build the bundled street map of the campus area from OpenStreetMap (ODbL).

    python tools/build_vector_map.py        # writes assets/data/vector_map.json

One free Overpass query at build time. The app draws these shapes itself (lib/services/vectormap.dart),
so the street map needs no tile server, no key and works offline. (c) OpenStreetMap contributors.

Water and green (with their holes) and the sea come from the raw region data of tools/fetch_region_raw.py,
assembled into proper rings with shapely, so run that first and use tools/.venv/Scripts/python.

File format (coordinates are integers: degrees * q):
  q          1000000
  bbox       [south, west, north, east] in degrees
  green      [[ring, hole, ...], ...]               parks, grass, forest; flat [lat, lng, ...] rings
  water      [[ring, hole, ...], ...]               lakes and sea
  buildings  [[...], ...]
  roads      [[class, lat, lng, lat, lng, ...], ...]   class: 0 major, 1 street, 2 service, 3 path, 4 steps
  rail       [[...], ...]
  names      [[name, lat, lng], ...]               street names (one label per street)

[road_levels.py]
Which road ways of OpenStreetMap lie in a tunnel and which on a bridge, shared by the map builders (build_vector_region.py, build_vector_outer.py, build_campuses.py, build_vector_map.py,
split_road_levels.py): the same idea as rail_visibility.py for the railways, which hides the tracks that are not seen from above.

A road in a tunnel (tunnel=yes, culvert, building_passage, flooded; covered=yes; location=underground; a negative layer without a bridge) is NOT a road of the surface: drawn as one it crossed the parks, the housing and the
water (the tunnels of Sodra lanken, Klaratunneln, Norrtullstunneln: 60 km of the major roads, 144 km of all the roads were drawn as ordinary streets: docs/AUDITORIA_3D_SUELO_16NOV.md G4, docs/AUDITORIA_2D_16NOV.md F07).
The maps keep it apart, in the key `road_tunnels` of their file, and draw it as a pale dashed ghost under the roads; a road on a bridge (bridge=yes, viaduct, movable...) goes to `road_bridges` and is drawn over the railways.
(c) OpenStreetMap contributors, ODbL.

[split_road_levels.py]
Moves the roads in a tunnel and the roads on a bridge of the detail maps of the campuses to their own keys, `road_tunnels` and `road_bridges`.

    tools/.venv/Scripts/python tools/split_road_levels.py [--cache tools/cache] [--check] [id ...]

The detail map of a campus (assets/data/vector_<id>.json, vector_map.json for the main campus) had every way of OpenStreetMap that has a `highway` in `roads`, a tunnel as an ordinary street: the tunnels
of Sodra lanken, Klaratunneln and Norrtullstunneln crossed the parks and the housing (docs/AUDITORIA_3D_SUELO_16NOV.md G4). The builders (build_vector_map.py, build_campuses.py) now keep them apart with
tools/road_levels.py; this brings the files that are already made up to date WITHOUT making them again (a new build drops what the later scripts added: the sea, the land use, the narrow gauge, the buildings of Overture...).
Each road of the file is found again in the raw answers of OpenStreetMap of tools/cache (detail_raw.json for the main campus, campus_<id>_geom.json for the others) by its coordinates, which the builders copy as they are,
and its tags say what it is. Safe to run again (a road that is already in `road_tunnels` or `road_bridges` stays there; nothing else of the file changes).
--check writes nothing and exits 1 when a file is not up to date. (c) OpenStreetMap contributors, ODbL.

[add_extract_lines.py]
Adds the piers, breakwaters and quays, the ferry routes and the runways of the extract of BBBike to the detail maps of the campuses, as the key `lines` of their files.

    tools/.venv/Scripts/python tools/add_extract_lines.py [--pbf tools/cache/bbbike/Stockholm.osm.pbf] [--check] [id ...]

The coarse maps of the region and of the outer ring carry these lines outside the boxes of the campuses (build_vector_region.py, build_vector_outer.py: tools/region_extras.py) and cut them at the edge of every box, because the detail map
of the campus draws its own box: so this puts the same lines inside the box (a pier of Djurgarden or of the harbour of Ersta, the ferry of Slussen), cut at the same edge, and a pier that crosses it is drawn on both sides.
`lines` is `[[kind, lat, lng, ...], ...]` (integers in millionths of a degree), kind 0 a pier, 1 a breakwater, a groyne or a quay, 2 a ferry route, 3 a runway, 4 a taxiway. Running it again replaces what it wrote before; nothing
else of the file is touched. The waterways of a box are the `streams` that tools/add_map_zones.py writes. --check writes nothing and exits 1 when a file is not what the extract gives.
Map data (c) OpenStreetMap contributors, ODbL.

[build_vector_region.py]
Build the coarse street map of greater Stockholm that surrounds the detailed campus map.

    python tools/fetch_region_raw.py           # once: raw OpenStreetMap shapes (cached)
    tools/.venv/Scripts/python tools/build_vector_region.py

Writes assets/data/vector_region.json. Everything is simplified (a few metres) and the campus box is cut
out of the areas and left out of the roads, because assets/data/vector_map.json draws it in detail. The railways of the file are kept as they are (add_narrow_gauge.py
and rails_clean.py worked on them; --from-scratch builds them from the raw answers). The waterways, piers, ferry routes and runways come from the extract of BBBike
(tools/fetch_region_pbf.py) when it is there (tools/region_extras.py).
(c) OpenStreetMap contributors, ODbL.

Format (integers are degrees * q):
  q, bbox [south, west, north, east], hole [south, west, north, east] (the campus box)
  water, green  [[ring, hole, ...], ...]  flat [lat, lng, ...] rings, biggest area first (water from 200 m2 and green from 1,000 m2)
  roads         [[class, lat, lng, ...], ...]   class 0 major road, 1 street; the roads of the surface
  road_tunnels, road_bridges   the same for the roads in a tunnel and on a bridge (tools/road_levels.py)
  rail          [[lat, lng, ...], ...]
  waterways     [[class, lat, lng, ...], ...]   class 0 river or canal, 1 stream, 2 ditch or drain
  lines         [[kind, lat, lng, ...], ...]    kind 0 pier, 1 breakwater or quay, 2 ferry route, 3 runway, 4 taxiway

[region_extras.py]
What the extract of BBBike (tools/fetch_region_pbf.py) adds to the coarse maps of the region and of the outer ring, which are made from Overpass answers that never had it: the waterways (streams, rivers,
canals, ditches), the piers, breakwaters and quays, the ferry routes, the runways and taxiways of the airfield, and the streets of the far ring (docs/OBRA_3D_SUELO.md; docs/AUDITORIA_3D_SUELO_16NOV.md G14, G11; docs/AUDITORIA_2D_16NOV.md F02, F09, F12, F13).

Used by build_vector_region.py and build_vector_outer.py (and add_extract_lines.py for the detail maps of the campuses). The extract covers 17.535 to 18.557 E and 59.103 to 59.571 N: the two strips of the frame outside it (west of 17.535 E, south of
59.103 N) get none of this (docs/OBRA_2D_REGION.md). Map data (c) OpenStreetMap contributors, ODbL.

Lines are given as `[class, lat, lng, lat, lng, ...]` (integers, 1/q degrees) for the keys of the vector files:
  waterways  class 0 a river or a canal, 1 a stream, 2 a ditch or a drain
  lines      kind 0 a pier, 1 a breakwater, a groyne or a quay, 2 a ferry route, 3 a runway, 4 a taxiway

[build_vector_outer.py]
Build the OUTER street map: the coarse ring around greater Stockholm that makes the covered area reach well past every campus.

    tools/.venv/Scripts/python tools/fetch_outer_raw.py     # once: the raw OpenStreetMap shapes (cached in tools/cache/outer)
    tools/.venv/Scripts/python tools/build_vector_outer.py

Writes assets/data/vector_outer.json, in the format of vector_region.json (see build_vector_region.py). It covers OUT (outer_geom.py)
except the region (vector_region.json draws that) and the detailed map of every campus outside the region (their own files draw
them). Around those campuses (MID_METERS) and in a band of 3 km round the region it is as fine as the region; further out it is coarser: where the extract of BBBike is
(tools/fetch_region_pbf.py: 17.535 to 18.557 E, 59.103 to 59.571 N) it has every street and every pond and park of 200 and 1,000 m2, with the geometry simplified to 8 to 10 m; in the two strips of the
frame outside the extract it is the same from the second extract (the frame cut out of the extract of Sweden: tools/strips.py, tools/cut_extract.py), and as coarse as it was, with the main roads, the railways,
and the water and green areas that are big enough to be seen on a map of the whole area, when that file is not there. The railways of the file are kept as they are
(--from-scratch builds them from the raw answers). The roads in a tunnel and on a bridge have their own keys, as in the region file.  (c) OpenStreetMap contributors, ODbL.

[strips.py]
The two strips of the frame of the maps that the extract of BBBike does not cover, and the SECOND extract that the builders read for them.

The frame of the 2D and 3D maps is 59.00 to 59.56 N, 17.30 to 18.45 E. The extract of BBBike (tools/fetch_region_pbf.py) covers 17.535 to 18.557 E and 59.103 to 59.571 N, so a strip of the frame
west of it (17.30 to 17.535 E, 680 km2) and one south of it (59.00 to 59.103 N, 750 km2) had no land use, no paths, no small streets, ponds or parks, no trees and no words of their own (docs/OBRA_2D_REGION.md,
docs/OBRA_3D_SUELO.md section 12). The second extract is the frame cut out of the extract of Sweden of Geofabrik with tools/cut_extract.py (the same data of OpenStreetMap, ODbL): about 1 GB downloaded by hand,
once, into a folder of its own, and a file of the frame that stays next to it (and a copy of it in tools/cache/geofabrik/, which is where the builders look; STRIPS_PBF says another place).

Every builder that reads the extract reads two files now, each over its own part of the frame, so that nothing is drawn twice and nothing is left out at the seam:

  primary   the extract of BBBike, over the part of the frame that it covers, 300 m in from its edge (an area that the extract cuts at its edge is not used: tools/build_vector_outer.py COVER)
  strips    the second extract, over the rest of the frame: the two strips and the 300 m inside the edge of the first, which is where the multipolygons that BBBike cuts are whole

Without the second file a builder reads the first over the whole frame as it always did, and says so: its asset has no strips then, and `--check` of one that has them says that it cannot check.
The windows are polygons of metres (x = longitude * KX, y = latitude * KY: tools/region_geom.py). Map data (c) OpenStreetMap contributors, ODbL.

[cut_extract.py]
Cuts a box out of a big extract of OpenStreetMap (a .osm.pbf), reading only files that are already on the disk: the frame of the maps out of the extract of Sweden.

    tools/.venv/Scripts/python -I tools/cut_extract.py --src C:/dev/osm_sweden/sweden-latest.osm.pbf --out C:/dev/osm_sweden/stockholm-frame.osm.pbf [--bbox south,west,north,east] [--check]

The box is the frame of the 2D and 3D maps (59.00 to 59.56 N, 17.30 to 18.45 E: tools/region_osm.py FRAME) unless --bbox says another. The rule is the one of `osmium extract` with its
strategy "complete ways", plus the multipolygons that touch the box with all their parts:

  nodes       the nodes with a tag that lie in the box (a shop, a tree, a place, a stop); the nodes of the ways come with their ways
  ways        every way with at least one node in the box, with ALL its nodes (also the ones outside: a way is never cut)
  relations   every relation of type `multipolygon`, `site` or `building` with a member (a node or a way) in the box, with all its member ways and their nodes (the lakes, the bays, the islands,
              the big buildings: tools/region_osm.py `read_areas` assembles them, and one with a part missing would be lost; the universities, stadiums, hospitals and churches that are
              `site` relations and the `building` relations: tools/build_map_labels.py `read_sites` gives them their word), and the relations of type `boundary` that are an area of the
              map (a nature reserve or a national park, or one with a `leisure`, `natural` or `landuse` tag: not the administrative boundaries, which are the size of a county)

The nodes that only help a way keep their tags (`remove_tags=False`), so that an old style multipolygon whose tags are on its ways is assembled as it is in the source. No other relation is kept
(routes, boundaries and the like: no builder of this repository reads them). The file is written by pyosmium (`osmium.BackReferenceWriter`: no other program is needed), in the order of the
format, and read back by `--check`, which counts what a builder needs (the closed ways with a `building` tag, the multipolygons, the nodes with a tag) and says where the first node is.

The source is never changed and never fetched here: tools/fetch_region_pbf.py is the one that downloads, and the extract of Sweden (https://download.geofabrik.de/europe/sweden-latest.osm.pbf, about 1 GB,
free) is downloaded by hand, once, into a folder of its own. Map data (c) OpenStreetMap contributors, ODbL.

[build_campuses.py]
Detailed maps, walking network and places for the campuses outside Valhallavägen.

    tools/.venv/Scripts/python tools/build_campuses.py [--refetch] [id ...]

For every campus of tools/campuses.py except the main one, one Overpass query for the shapes (buildings, roads, rail,
water, green) and one for places and address points; the answers are cached in tools/cache/ so the build can be repeated.
The places are asked for over the places area of the campus (its box and, when it has a `reach`, the circle round its centre: tools/place_classes.py), and the classes
of place are the ones of tools/place_classes.py (food, services, and since the audit of 16 Nov 2026 the shops, pharmacies, cash machines, post and parcel points, drinking water
and bike shops). tools/add_places.py gives the same file from an extract of OpenStreetMap, with no server.
Writes assets/data/vector_<id>.json (the same format as vector_map.json), merges the walking network of every
campus into assets/data/walk_graph.json, the places into assets/data/campus.json and the campus list into
assets/data/campuses.json and lib/services/campuses_list.g.dart (the list of tools/campuses.py is the only one: the app and the tests
read what is written from it). (c) OpenStreetMap contributors, ODbL.
The SEA is no polygon of OpenStreetMap (it is the water on one side of the coastline), so the maps of this script have none: after a build run
tools/add_sea_to_details.py (it puts the sea of the box back; `--check` says whether a file lacks it; docs/OBRA_MAPA_BASE.md).

    tools/.venv/Scripts/python tools/build_campuses.py --list-only    # only the two files of the list
    tools/.venv/Scripts/python tools/build_campuses.py --edge-first            # writes only the key `edgeFirstBoxes` of the walking network (the campuses that are not KTH's)
    tools/.venv/Scripts/python tools/build_campuses.py --fix-places frescati   # puts the places of the buildings of that campus (never KTH's) whose point is
                                                                      # outside their building, or in a smaller one, into it (tools/place_interior.py)
    tools/.venv/Scripts/python tools/build_campuses.py frescati       # ADD the campus: its map file, its piece of the walking network
                                                                      # and its places; nothing of the other campuses is touched
    tools/.venv/Scripts/python tools/build_campuses.py                # everything again from the caches (drops what later scripts added)
    tools/.venv/Scripts/python tools/build_campuses.py --check        # writes nothing; exit 1 when the two files are not what the list makes

[build_walk_graph.py]
Build the on-device walking graph of the campus area from OpenStreetMap (ODbL).

    python tools/build_walk_graph.py       # writes assets/data/walk_graph.json

One-off, free: a single Overpass query at build time. The app then routes on the phone (no routing
server, works offline). Data (c) OpenStreetMap contributors, ODbL: keep the attribution in the app.

File format (compact, integers):
  bbox   [south, west, north, east] in degrees
  nodes  flat list [lat1e6, lng1e6, ...]
  edges  flat list [a, b, lengthDm, flags, ...]   a/b = node indexes, length in decimetres,
                                                  flags = 1 for stairs, else the surface, wheelchair, incline and trail bits
                                                  of tools/walk_access.py

[build_osm_doors.py]
The entrances of every building of every campus: assets/data/doors.json.

    tools/.venv/Scripts/python tools/build_osm_doors.py [--refetch] [--replace] [campus_id ...]

With campus ids it ADDS the buildings of those campuses to the file and leaves every entry that is already there as it is (the way the other scripts of
the chain take ids): a guessed entrance that was made with another walking network is not made again, so adding a campus moves no door of another.
With --replace the entries of those campuses that are in the file are made again, each in the place where it was; the entries of every other campus stay as they are.
With --guesses-only (and ids) only the GUESSED entrance of the buildings of those campuses that have no other is made again: no row is added or taken away, no real door,
no outline and no flag of any row moves (`keep_real_doors`). Without ids it makes the whole file again (then run tools/apply_door_overrides.py).

Sources, for each building drawn on the map (ways, buildings with a courtyard and the ones added from Overture):
  1. the entrance nodes of OpenStreetMap (entrance=main|yes|home|residential|staircase|shop|service|garage...)
     within 6 m of its outline: real doors;
  2. a building with no entrance in OpenStreetMap gets the most likely one: the point of its outline nearest to
     the walking network (a path or a street), marked as a guess; a point that lies inside another building (by more than
     INSIDE_OTHER_M: a building drawn inside the outline of a bigger one) is not taken while a better one is left.
     In the campuses of V2_CAMPUSES a learned ranker comes first (tools/guess_door_v2.py with the model tools/door_model_v2.json): it scores the points of the outline
     with what the walking network and the streets in front of the wall, the shape, the address points and the doors of the neighbours say; also marked as a guess.
Entrances that are private (access=private|no), emergency exits and plain exits are left out.

File: {"q": 1000000, "b": [[lat, lng, areaM2, kth, ring, doors], ...]} in 1/q degrees; ring = flat [lat, lng, ...];
doors = [[lat, lng, kind], ...] with kind 0 main, 1 door, 2 service, 3 guessed; kth = 1 when the building is one of
the KTH buildings with floor plans (the app uses the entrances of its own catalogue for those).

[build_door_refs.py]
The house letters of the entrances of a building that has several: assets/data/door_refs.json.

    tools/.venv/Scripts/python tools/build_door_refs.py          # writes the file (reads only the caches and doors.json: no network)
    tools/.venv/Scripts/python tools/build_door_refs.py --check  # writes nothing; exit 1 when the file is not what the caches make

A large building of a university is one polygon with several entrances, each of a house: Södra huset of Stockholm University has the entrances 10A to 10F on
one side and 14A to 14F on the other (the `addr:housenumber` of the entrance nodes of OpenStreetMap), and the rooms of the timetable are written with the
letter of their house ("D299"). The app picks the entrance of the house of the room (`OsmDoors.doorFor(..., wing: 'D')`) instead of the one nearest to the
person, which can be 200 m of corridors from the room. This file says which entrance node has which letter.

Only the entrances of the campuses whose table of aliases writes the house of a room (`WING_CAMPUSES`: Frescati; the doors of KTH are not touched), and only
those of a building whose entrances carry two letters or more (the "12A" and "12B" of a block of flats are not houses of one building: a campus in the
middle of the city, like Ersta, has dozens of them, which no room refers to). The letter is the one after the number of `addr:housenumber`, or a `ref`
of one letter. Format: {"q": 1000000, "r": [[lat, lng, "D"], ...]} in 1/q degrees, sorted. ODbL, like doors.json.

[build_campus_snapshot.py]
Bundle a snapshot of the OpenStreetMap places of the MAIN campus (Valhallavagen), with the same query and the same conversion as CampusData in
lib/services/data.dart (tools/place_classes.py), so the map works offline and on first launch even when Overpass is slow or rate limited.

    tools/.venv/Scripts/python tools/build_campus_snapshot.py [--out PATH]

This is the FIRST step of the chain that makes assets/data/campus.json: it writes the places of the main campus ALONE (to assets/data/campus.json, unless --out says another
file), and tools/build_campuses.py then adds those of the other campuses to it (it keeps a copy of this one in tools/cache/campus_main.json). On the file that is in the repository
(all campuses) use tools/add_places.py instead, which needs no server: running this over it would take the other campuses away. To renew the copy that build_campuses.py starts
from: `--out tools/cache/campus_main.json`, then `tools/build_campuses.py` (which drops what the later scripts added: tools/add_places.py puts the new classes back).

The places area is the WHOLE box of the campus (the map is 3.0 x 3.4 km; the snapshot of before 16 Nov 2026 came from a box of 1.8 x 1.7 km and left out most of the food places
of Ostermalm, 462 of them) and the circle of its `reach` (tools/campuses.py); the classes are the ones of tools/place_classes.py: named buildings and rooms, food, services,
and the shops, pharmacies, cash machines, post and parcel points, drinking water and bike shops (audit IP-03 and IP-06, docs/OBRA_LUGARES.md).
(c) OpenStreetMap contributors, ODbL.

[place_classes.py]
The classes of place that the app lists, and how OpenStreetMap writes each one: ONE table, read by build_campuses.py (the Overpass query of the places and the
conversion of an element into a place), build_campus_snapshot.py, add_places.py (the same conversion over an extract of OpenStreetMap), build_hours.py and
places_audit.py. The Dart side is `serviceSubtype` and `kServiceAmenities` of lib/services/data.dart and lib/services/shops.dart;
test/place_classes_test.dart keeps both equal. (c) OpenStreetMap contributors, ODbL.

A place of campus.json is {id, name, lat, lng, kind, ref, subtype, level, address} (and `info` for the toilets). `kind` is one of
  service   a point that gives a service: what `service_subtype` says it is (toilets, a charger, a defibrillator, a rack, a pharmacy, a shop...)
  food      amenity=restaurant|cafe|fast_food (a cafe that is also a bakery stays food: it is where the person looks for it)
  room      indoor=room|area with a name
  building  a building with a name (and the amenities that are none of the above, which an old snapshot called "building")
The classes that are NOT a room or a building (the services) and the words that name them in the app:

  subtype            OpenStreetMap                                    Nearby chip           text
  toilets            amenity=toilets                                  Toilets               svc_toilets
  charging_station   amenity=charging_station                         Services              svc_charging
  defibrillator      emergency=defibrillator                          Services              svc_aed
  first_aid_kit      emergency=first_aid_kit                          Services              svc_aed
  first_aid          amenity=first_aid                                Services              svc_aed
  bicycle_parking    amenity=bicycle_parking                          Bike parking          svc_bike
  bicycle_repair_station  amenity=bicycle_repair_station              Services              svc_repair
  drinking_water     amenity=drinking_water                           Services              svc_water
  bicycle_shop       shop=bicycle                                     Services              svc_bike_shop
  supermarket        shop=supermarket                                 Shops                 svc_supermarket
  convenience        shop=convenience                                 Shops                 svc_convenience
  kiosk              shop=kiosk                                       Shops                 svc_kiosk
  greengrocer        shop=greengrocer                                 Shops                 svc_greengrocer
  bakery             shop=bakery | shop=pastry                        Shops                 svc_bakery
  pharmacy           amenity=pharmacy                                 Pharmacy              svc_pharmacy
  atm                amenity=atm                                      Cash and post         svc_atm
  post_office        amenity=post_office                              Cash and post         svc_post
  parcel_locker      amenity=parcel_locker                            Cash and post         svc_parcel
  (the second round, 21 Nov 2026, docs/OBRA_LUGARES.md section 10: what a student looks for beside the first list)
  library            amenity=library                                  Study spot            ptype_library
  copyshop           shop=copyshop | shop=printing                    Printer / mail        svc_copyshop
  stationery         shop=stationery                                  Books and stationery  svc_stationery
  bookshop           shop=books                                       Books and stationery  svc_bookshop
  bank               amenity=bank                                     Cash and post         ptype_bank
  bureau_de_change   amenity=bureau_de_change                         Cash and post         ptype_bureau_de_change
  laundry            shop=laundry | amenity=laundry                   Laundry               svc_laundry
  dry_cleaning       shop=dry_cleaning                                Laundry               svc_dry_cleaning
  compressed_air     amenity=compressed_air                           Services              svc_air
  clinic             amenity=doctors | amenity=clinic | healthcare=doctor | healthcare=centre     Health   ptype_clinic
  dentist            amenity=dentist | healthcare=dentist             Health                ptype_dentist
  hospital           amenity=hospital                                 Health                ptype_hospital
  police             amenity=police                                   Services              ptype_police
  metro_station      railway=station + (station=subway | subway=yes)  Metro and train       svc_metro
  train_station      railway=station | railway=halt (the rest)        Metro and train       ptype_train_station
  tram_stop          railway=tram_stop                                Metro and train       svc_tram_stop

`info` of a toilet: words separated by a space, `fee` (fee=yes), `nofee` (fee=no) and `customers` (access=customers), nothing when OpenStreetMap says none of them;
a toilet with access=private or access=no is not a place of the app (it is not for the public).

[add_places.py]
Completes assets/data/campus.json from an extract of OpenStreetMap: the places that the Overpass queries of the first builds did not ask for, and the places of the area that the map
draws, no network.

    tools/.venv/Scripts/python tools/add_places.py [--pbf PATH] [--check] [--report]

What it adds (the audit of the places of 16 Nov 2026, docs/AUDITORIA_INTERIORES_LUGARES_16NOV.md, IP-03, IP-06 and IP-14; docs/OBRA_LUGARES.md):
  * the places of the WHOLE box of Valhallavagen (the map is 3.0 x 3.4 km, and the places came from a box of 1.8 x 1.7 km: Ostermalmstorg had no food place of 124);
    (the racks to leave a bike are not added: bike_parking.json, built by build_bike_parking.py from the boxes of the campuses, has them)
  * the places of the REACH of a campus (`reach` of tools/campuses.py: 1.5 km round Ersta and Frescati, whose boxes are tight), food and services only;
  * the new classes of tools/place_classes.py, in every box: shops (supermarket, convenience store, kiosk, greengrocer, bakery), pharmacies, cash machines, post offices, parcel
    lockers, drinking water and bike shops;
  * the `info` of a toilet: whether it is paid and whether it is for customers (see tools/place_classes.py), and a toilet that is not for the public is taken out.
  * the `wheelchair` tag of the places of the app that OpenStreetMap gives one and that assets/data/access.json (built by tools/build_access.py from the boxes) does not have yet: the
    new places lie outside the boxes (a third of the food places of the reach of Ersta say whether they are accessible), and a place says "no accessibility data" when the file is silent;
A place of the file that is one of the new classes in OpenStreetMap (an old snapshot called a pharmacy "building" and a store of Kista Galleria "room") becomes that class and keeps its
name and its place. No other place is touched, moved or taken out, and the order of the file is kept: the new ones come after the old, in the order of their ids. Running it again
changes nothing; `--check` writes nothing and exits 1 when the file is not what it makes, `--report` says what was added where.

After it, run `tools/build_hours.py --pbf PATH` (the opening times of the new places) and `tools/pack_osm_data.py`. The Overpass way to the same file is build_campuses.py (its
query asks for the same classes over the same areas, tools/place_classes.py): this script is the one that needs no server. Place format: tools/place_classes.py. (c) OpenStreetMap
contributors, ODbL.

[fetch_osm_addr.py]
Download OpenStreetMap address points (street + house number) around the KTH campuses: in Swedish
data they sit at the entrance of the address, which tells where a building's main door is.
Free Overpass query (ODbL); cached in tools/cache/osm_addr.json.

[build_building_labels.py]
Names for the buildings of the map: assets/data/building_labels.json.

    tools/.venv/Scripts/python tools/build_building_labels.py

Every label is a real name, never a code like "43:15". Sources, in this order for each building:
  1. KTH buildings (the 57 whose floor plans are placed on the map): the name KTH Places gives ("Kemi",
     "Maskin", "Q-huset"), put on the footprint of the hand-fitted plans;
  2. the OpenStreetMap name ("KTH: K" becomes "K-huset", "Karolinska..." stays);
  3. the address of the building from OpenStreetMap address points ("Valhallavägen 79", a range like
     "Villagatan 19-21" when several numbers belong to it). Residential blocks are labelled this way.
A building with none of these has no label (nothing is invented).

File: {"labels": [[text, lat, lng, rank, areaM2, key]...]}  rank 0 = KTH name, 1 = OSM name, 2 = address, 3 = nearest address;
key = KTH building key for rank 0, else "".

[build_gyms.py]
Builds assets/data/gyms.json: gyms and outdoor gyms (utegym) near the KTH campuses.

Sources:
  osm   OpenStreetMap (ODbL, (c) OpenStreetMap contributors): leisure=fitness_centre, leisure=fitness_station,
        amenity=gym, sport=calisthenics/crossfit/fitness; within a radius of each campus (`near` of tools/campuses.py).
  kth   Facts taken from published pages and placed by hand:
        - KTH-Hallen is a gym of Nordic Wellness in a hall owned by the KTH sports foundation (Brinellvagen 38).
        - Akademiska Hus' property map of KTH Campus (Elsparkcyklar.pdf, the same drawing as the scooter places) shows an
          "UTEGYM" symbol north of KTH-Hallen; its position is placed on the real map with the same fitted transform
          (about 15 m of error).
        - Akademiska Hus (24 March 2022): the Albano outdoor gym is at Bjornasplatsen, where Roslagsbanan crosses Greta
          Arwidssons vag, and is open to everyone (the OpenStreetMap station there takes its name).

Record: [lat, lng, kind (0 gym, 1 outdoor gym), name, access (0 unknown, 1 free, 2 paid, 3 members), website, source (0 osm, 1 kth)]
        and, for the places in HOURS: times (OpenStreetMap syntax), 1 (read on an official page), day read, page, note

WHAT IS IN THE FILE (10 Nov 2026, and 16 Nov 2026 for Ersta and Frescati): only places CONFIRMED against a page of their operator or owner that is current (CONFIRMED below,
with the evidence of each; the list of what was dropped and why is docs/GYMS_VERIFICATION.md). OpenStreetMap is only used to FIND
candidates: the ones that are not confirmed are printed and are NOT in the app (gyms close and move: of the 53 places this script
once took from OpenStreetMap, 20 could not be confirmed, 5 of them are gone for sure, FLX Fitness among them). To add a place,
check it on its own page and add a line to CONFIRMED.

TIMES (13 Nov 2026): only where the official page states them for THAT place; the app shows what it cannot read as text and says nothing
where there are none (docs/OPENING_HOURS.md). Left out on purpose: clubs whose page lists ranges it does not label, ranges with no days, and
anything seen only through a search snippet.

Usage: python tools/build_gyms.py [--offline]   (--offline does not ask OpenStreetMap for candidates: the file is the same)

[build_hours.py]
Builds assets/data/hours.json: when the places of the map are open, and their web pages.

    python -X utf8 tools/build_hours.py [--refetch] [--today YYYY-MM-DD]
    python -X utf8 tools/build_hours.py --offline        (no network: the OpenStreetMap part of the file is kept as it is, and the part
                                                          read on official pages, tools/hours_confirmed.json, is put over it again)
    python -X utf8 tools/build_hours.py --pbf [PATH]     (no server: the same answers read from an extract of OpenStreetMap, tools/osm_places.py; the
                                                          day of the data is the one of the extract. Without PATH: tools/cache/bbbike/Stockholm.osm.pbf)

Sources
  osm      The `opening_hours` and `website` tags of the OpenStreetMap elements of assets/data/campus.json (ODbL, (c) OpenStreetMap
           contributors), asked from Overpass by id (the places of the file are not searched again: their ids are the ones of the app).
           The date of a record is the day a mapper says they checked the times (tags check_date:opening_hours, check_date, survey:date)
           and, when there is none, the day of the last edit of the element.
  checked  tools/hours_confirmed.json: times read on the OFFICIAL page of a place (its own site, KTH, THS, the company that runs it) with
           the address of the page, the text it showed and the day it was read. They replace what OpenStreetMap says for that place.
           These facts are not part of the ODbL offer (the field "sources" of the file says so, as gyms.json does).
           A "suppress" list in the same file names the places whose OpenStreetMap times are taken away (an official source disagrees and it
           is not clear which is right).
  The gyms (assets/data/gyms.json) carry their own times, built by tools/build_gyms.py.

Record of a place, by its id ("node/123"): [times, source, date, web page, note]
  times   the value of `opening_hours` in the syntax of OpenStreetMap ("Mo-Fr 10:00-22:00; PH off"), or "" (only the web page is known)
  source  0 OpenStreetMap, 1 an official page
  date    YYYY-MM-DD, see above
  web     https address of the page of the place, or "" (for an official source: the page where the times were read)
  note    what the official page said that does not fit in the times (staffed hours, term time...), or "": NOT a sentence but lines of the
          vocabulary of the app ([["lunch", {"t": "10:30-13:30"}]], see tools/hours_note.py), so that it shows in the language of the person
The app reads the times exactly or not at all (lib/services/openinghours.dart): what it cannot read it shows as text, and never says
"open" or "closed" about it.

The file also has "built" (the day it was made), "osm_base" (the moment of the OpenStreetMap data of the answers), "sources", and what it was asked about:
"asked" (how many places of assets/data/campus.json) and "asked_sha" (the first 16 digits of the SHA-256 of their ids, sorted, one to a line). A place that is in
campus.json and was never asked about has no record, and a record says nothing of that: tools/data_audit.py (D-HRS-012) fails when campus.json is not the file this
one was made for, which is how the food places of two campuses went without times for three days (audit IP-04, docs/OBRA_LUGARES.md).

[build_scooter_parking.py]
Builds assets/data/scooter_parking.json: where an electric scooter may be parked near the KTH campuses.

Sources (all free to reuse):
  city  Stockholms stad, Trafikkontoret, open data "Elsparkcykelplats" (WFS od_gis:Elsparkcykelplats): the places the
        city has marked for shared e-scooters. Reuse is free and licence-free (https://openstreetgs.stockholm.se).
  kth   The five areas Akademiska Hus set up at KTH Campus (Valhallavagen) for e-scooters, from the map "Elsparkcyklar.pdf"
        of their news post "Ny parkering for elsparkcyklar" (17 Oct 2022). The map is a drawing turned about 53 degrees
        from north: its numbered arrows were placed on the real map with a similarity transform fitted on eight buildings
        (about 15 m of error), so their positions are approximate.
  osm   OpenStreetMap, amenity=scooter_parking / kick-scooter_parking (ODbL, (c) OpenStreetMap contributors).

Only spots near a campus are kept. Two places closer than SAME_PLACE_M with the same address are one place (the city's layer has
two or three points for one marked area): the newest month is kept. Usage:
    python tools/build_scooter_parking.py [--city-file esp.json]   (without it the city layer is downloaded)
    python tools/build_scooter_parking.py --merge-only             (no download: the same-place merge on the file as it is)

[build_access.py]
Builds assets/data/access.json: what OpenStreetMap knows that helps people with reduced mobility, in the boxes of the campuses.

    python tools/build_access.py            # fetch (cached in tools/cache/access_<id>.json) and write the file
    python tools/build_access.py --refetch  # ask Overpass again

What it keeps (one Overpass query per campus box, free):
  spots   [[lat, lng, kind, status, name], ...]  kind:
            bench               amenity=bench: a place to rest
            accessible_toilets  amenity=toilets with wheelchair=yes|designated or toilets:wheelchair=yes
            disabled_parking    parking for disabled people (capacity:disabled, parking_space=disabled)
            elevator            highway=elevator (a lift outdoors, at a station or a footbridge)
            accessible_entrance an entrance or a door with wheelchair=yes|designated, or an automatic door
          status  the wheelchair tag of any other place or building that has one: {"node/123": "yes" | "limited" | "no"}, so that
                  a place the app already shows can say whether it is accessible.
The file says what the data says: a place with no tag is NOT in `status` (unknown is not the same as not accessible).
Data (c) OpenStreetMap contributors, ODbL: the file is offered in the open data zip (tools/pack_osm_data.py).

[institution_places.py]
The places of the institutions that the snapshot of OpenStreetMap (assets/data/campus.json) does not have: buildings that OpenStreetMap draws and does not name, or that lie
outside the box of the snapshot of the main campus. Writes assets/data/institution_places.json, which the app merges into its places (lib/services/institutionplaces.dart):

    tools/.venv/Scripts/python tools/institution_places.py            # writes the file
    tools/.venv/Scripts/python tools/institution_places.py --check    # writes nothing; exit 1 when the file is not what the list makes

WHY: the rooms of an institution are placed by the building that its timetable system says (the tables of aliases, lib/services/aliases.dart), and a building that is not a place
of the app cannot be a destination (docs/EXPANSION_ESTOCOLMO_ETAPA_2.md, 10.5 and 11 point 5). The ids are those of the objects of OpenStreetMap, so that nothing changes when the
snapshot grows to hold them: a place of the snapshot with the same id is replaced by this one (the name below is the one the institution writes).

Each entry: id (OpenStreetMap), name (as the institution writes it; the naming policy of docs/EXPANSION_ESTOCOLMO_ETAPA_1.md 2.4: the full name of an institution, never
"SU", "HHS", "SSE" or "Karolinska" alone), campus (tools/campuses.py: the position must lie in its box), inst (the institutions that use the place), subtype (a type of place the
app has a text for), address and note (where the fact comes from). The position is the middle of the object in the raw answers that the builders of the maps cached
(tools/cache): the centre of the outline for a way, the middle of the box for a relation (an approximation, said in `approx`).

[build_bike_parking.py]
Builds assets/data/bike_parking.json: where a bike, an e-bike or an e-scooter can be left near the KTH campuses, with what
OpenStreetMap says about each place (a roof, lockers, how many bikes, a charger for e-bikes, a fee, who may use it).

Source: OpenStreetMap, amenity=bicycle_parking (nodes, and ways and relations as their centre) and the charging stations for
bikes (amenity=charging_station with bicycle=yes). (c) OpenStreetMap contributors, Open Database Licence (ODbL) 1.0. The
places for e-scooters are not here: they stay in assets/data/scooter_parking.json (tools/build_scooter_parking.py).

Area: every campus box of assets/data/campuses.json plus 800 m on every side (a campus inside another one, AlbaNova, is in the
box of the other). One Overpass query per box, one at a time, with a pause between them and growing waits when the server is
busy (it often answers 502): `--max-wait-minutes` (default 60) is how long one query is retried before giving up on it. If a
box cannot be had, `--cache-dir` (a folder of earlier Overpass answers, any file with a list of elements or {"elements": [...]})
and, last, the plain places of assets/data/campus.json (position only, no tags) are used for that box, and the file says so in
"sources" and "fallback". User-Agent: campusmap-build/0.1. Usage:

    python tools/build_bike_parking.py [--out FILE] [--raw-dir DIR] [--max-wait-minutes N] [--cache-dir DIR] [--self-test]

THE FORMAT of assets/data/bike_parking.json (version 1):

    {"v": 1, "built": "YYYY-MM-DD", "sources": {...}, "fallback": [...],
     "spots": [[lat, lng, kind, flags, capacity, "n123", "label"], ...],
     "private": ["n456", ...]}

  lat, lng   degrees, six decimals (about 0.1 m)
  kind       the index of ParkingKind (lib/services/parking.dart): 0 a rack in the open (stands, wall loops, bollards...),
             1 a rack under a roof (a shelter: covered=yes, bicycle_parking=shed), 2 lockers (boxes), 3 inside a building or
             underground (bicycle_parking=building, indoor=yes, a bike garage). The places marked for e-scooters are another file
  flags      a sum of: 1 covered (under a roof, or inside), 2 secure (lockers, cameras, watched), 4 charging (a place to charge an
             e-bike), 8 fee (paid), 16 restricted (customers, permit holders, residents: not a place for everybody), 32 roof known
             (OpenStreetMap says yes or no about a roof: without it "not covered" only means "not known" and the app says nothing)
  capacity   how many bikes, 0 when OpenStreetMap does not say
  id         "n123" is node 123, "w456" way 456, "r789" relation 789 of OpenStreetMap (the app joins it to the same place of
             campus.json, whose id is "node/123")
  label      the name when the place has one that says more than "bicycle parking"; "" otherwise
  private    the ids (as above) of the racks OpenStreetMap says are private: they are not in "spots", and the app removes a rack
             with one of these ids from the plain racks of campus.json (which has them as racks like any other)

What is left out: access=private (private for sure, unless bicycle=yes or designated says bikes are welcome; listed in "private"). What is kept and
flagged 16: access=customers, permit, no, delivery, destination, residents, members, employees. Two places closer than 3 m are
one place (the richer one stays, its flags and its largest capacity too). A charging station for bikes within 10 m of a rack is
a charger of that rack (flag 4); alone it is a rack of its own.

[map3d/tiles3d.py]
Assembles the 3D map's tiles (assets/map3d/map3d.pmtiles) from the stage files of the other builders.

    tools/.venv/Scripts/python tools/map3d/tiles3d.py [--stage <folder>] [--out <file>] [--zmin 6] [--zmax 15] [--check]

reads   <stage>/<campus>/{ground,labels,buildings}.geojson   ground3d.py and buildings3d.py (the campuses, fine)
        <stage>/region.geojson                                ground3d.py (greater Stockholm and the outer ring, coarse)
        <stage>/inner/*.geojson                               buildings of the whole inner region (regionbuildings.py), when there are any
writes  assets/map3d/map3d.pmtiles                            one archive, vector tiles z6 to z15 (the engine zooms in from z15)

Layers of a tile: `ground` (t: w water, g green, z zone with c, r road with c 0 major .. 4 steps, u road in a tunnel and b road on a bridge (same c), l railway, m minor railway, s waterway with c, x other line with c, n street name with its rank r, p place name with its kind k),
`building` (h top, b base, k kind, lm landmark; the id of the feature is the OpenStreetMap id), `band` (the ribbons of glass round a tall tower: b, h; the id of its building), `bname` (the names of the buildings: n, rank, area, key).

[map3d/tiler.py]
A small vector-tile builder for the 3D map: features in longitude/latitude in, a PMTiles file of gzip-compressed Mapbox vector tiles out.

No server, no external tool (no tippecanoe, no tilemaker): shapely does the cutting and the simplifying, this file writes the protobuf of the tile and the
`pmtiles` package writes the archive. The engine of the map (MapLibre Native 13.7, the plugin `maplibre_gl` 0.28) reads the archive from the app's own
assets, offline (`pmtiles://asset://flutter_assets/assets/map3d/map3d.pmtiles`).

A feature is a `Feat`: a shapely geometry in (lng, lat), the properties, the name of its layer, the first zoom at which it is drawn (`minzoom`) and whether
it is `keep`t whole at every zoom of the pyramid from `keep_from` (the buildings of a campus must not vanish when the camera backs off one step, from zoom 14).

The pyramid, per zoom z:
  * a feature with `minzoom` <= z (and `maxzoom` >= z, when it has one) is drawn; a polygon smaller than 2 pixels (of a 512-pixel tile) is dropped unless it is `keep`t or the zoom is the top one;
  * the geometry is simplified to half a pixel (an eighth of a pixel, one unit of the tile, at the top zoom, where a building keeps its corners);
  * what lies in a tile is cut with a margin round the tile (80 units of 4096): the walls of an extruded building are not drawn along an edge that lies
    outside the tile (maplibre-native `isBoundaryEdge`), so a building that crosses a tile edge shows no wall inside itself.
(c) OpenStreetMap contributors, ODbL, for the data that goes through it.

[map3d/build_extras.py]
The trees and the decks of the bridges of the ground of the 3D map, from the extract of OpenStreetMap (audit findings G13 and G6, docs/OBRA_3D_SUELO.md section 14).

    tools/.venv/Scripts/python tools/map3d/build_extras.py [--pbf tools/cache/bbbike/Stockholm.osm.pbf] [--out tools/map3d/data/extras3d.json] [--check | --stats]

reads   the extract of BBBike (tools/fetch_region_pbf.py): the nodes `natural=tree`, the ways `natural=tree_row` and the areas `man_made=bridge`; and, over the two strips of the frame that it does not
        cover, the second extract (the frame cut out of the extract of Sweden: tools/strips.py, tools/cut_extract.py), when it is there
writes  tools/map3d/data/extras3d.json   {"q": 1000000, "trees": [[lat, lng], ...], "decks": [[ring, courtyard, ...], ...]} (integers in 1/q degrees, a ring as [lat, lng, lat, lng, ...] like the
        `water` of the vector files), read by tools/map3d/ground3d.py: the trees are points of the tiles of zoom 15 (a tree row gives a point every 7 m along it), the decks are flat polygons under the roads on a bridge

It is made for the 3D map only (the 2D map does not draw them), so it is not an asset of the app but a file of the tools; the archive that it goes into is offered under the ODbL (tools/pack_osm_data.py,
`map3d.pmtiles`). --check builds again from the extract, in memory, and exits with 1 when the file of the repository is not that. Map data (c) OpenStreetMap contributors, ODbL.

[build_region_buildings.py]
Builds the buildings of the whole area of the 2D map, outside the boxes of the campuses, as ONE compact binary asset.

    tools/.venv/Scripts/python tools/build_region_buildings.py [--cache tools/cache] [--pbf <extract>] [--out assets/data/vector_region_buildings.bin] [--check | --verify | --stats]

The detail map of a campus (assets/data/vector_<campus>.json) has the buildings, and the coarse maps of the region and of the outer ring have none, so the 2D map showed a
rectangle of buildings, with their fill and their edge, in the middle of streets with no building at all (docs/OBRA_2D_REGION.md). This is the data that closes it: every building of
OpenStreetMap of the frame of the 2D map that is not wholly inside the box of a campus (a building that crosses the edge of a box is kept, so that nothing is lost at the seam: drawn
twice it looks the same).

reads   <cache>/inner_buildings/cell_*.json, ring_*.json   the buildings that are ways, one answer of Overpass per cell of tools/map3d/fetch_inner_buildings.py (the inner region,
                                                            and with --ring the rest of the frame of the outer map; not in git, the same files that the 3D map is built from)
        <pbf>  (default <cache>/bbbike/Stockholm.osm.pbf)  the buildings that are relations (a big building with courtyards: 620 in the extract, 480 with courtyards), which the
                                                            cells do not have: tools/fetch_region_pbf.py; without the extract they are left out (`--no-relations`)
        <strips pbf> (tools/strips.py)                      the same relations in the strips of the frame that the extract of BBBike does not cover (the second extract: the frame cut out
                                                            of the extract of Sweden, tools/cut_extract.py): a relation belongs to the extract of the window where its first point lies
writes  assets/data/vector_region_buildings.bin             read by lib/services/region_shapes.dart; nothing else of assets/ is touched

--check   builds again from the cache, in memory, and exits with 1 when the asset in the repository is not that (a guard for a new download of the cells)
--verify  needs no cache: decodes the asset, and exits with 1 when it is damaged or breaks a rule that the app relies on (the same rules as test/region_shapes_test.dart, written
          again here in Python so that the format has two independent readers)
--stats   what is in the asset

The format is the one of tools/region_pack.py (shift 27: 0.15 m). Map data (c) OpenStreetMap contributors, ODbL.

[region_pack.py]
The binary container of the shapes of the 2D map outside the boxes of the campuses: the buildings (build_region_buildings.py) and the land use (build_region_zones.py) as polygons, and the paths
(build_region_paths.py) as open lines.

The detail map of a campus (assets/data/vector_<campus>.json) has the buildings and the land use, and the coarse maps of the region and of the outer ring have
none, so the map used to end them in a straight line at the edge of every box (docs/OBRA_2D_REGION.md). These files carry them for the rest of the map. They are read
by lib/services/region_shapes.dart, which keeps them as they are and reads the shapes of a tile when it is painted. All integers are little-endian; `varint` is the LEB128 of
Protocol Buffers; `zz` is a signed number as zigzag. The coordinates are those of the Web Mercator map, 0..1 from the west and from the north, as whole numbers of
2**-shift of the world (shift 27: 0.15 m at 59.3 N; shift 26: 0.3 m).

  header   56 bytes   magic (4 bytes: "RBLD" the buildings, "RZON" the land use, "RPTH" the paths), version 1, shift, cell_shift, flags (bit 0: each shape has a class; bit 1: the shapes are
                      open lines, see below), then u32:
                      col0, row0, cols, rows (the grid of cells: a cell is 2**cell_shift units, 1024 units = a tile of zoom 17 at shift 27), shapes, used_cells,
                      area_count, max_vertices, table_len, areas_len; then u64 digest of everything that is drawn
  table    used_cells x (varint: index of the cell minus that of the one before, varint: bytes of its block); index = row * cols + col, both from the origin
  areas    area_count x (varint len, then len bytes: varint minx, miny, width, height, [class when flag 0], rings, then the rings, each as below with its first vertex
           counted from (minx, miny)): the shapes with a hole, and the ones whose box is wider or taller than a cell. The app scans them all by their box
  blocks   the cells in the order of the table; a block is its shapes one after another, each ONE ring: [class], varint vertices, varint y0 - the y of the shape before
           (the cell's for the first), varint x0 - cell x, then (vertices - 1) times zz dx, zz dy from the vertex before. The shapes of a block are ordered by their
           first vertex (y, then x). A shape is listed in the cell of the corner of its box (the minimum of x and of y) and its box is not wider or taller than a cell:
           a query for a box looks at the cells from one before its first to its last

Open lines (flag bit 1, the paths): a shape is ONE open polyline of at least 2 vertices, stored as a ring is (varint vertices, y0, x0, then the steps) but NOT started at its top vertex, not wound,
with no courtyard, and the shapes of a block are ordered by their FIRST vertex (y, then x). A line is cut at the edges of the cells of the grid (`split_at_cells`: every piece lies in one cell, and
consecutive pieces share the point where they meet), so that none is wider or taller than a cell and the list of areas is empty; a query reads the cells that its box touches and the piece is then drawn
as an open line (the painter must not close it).

A ring is closed without the repeated closing point and starts at its top vertex (least y, then least x). The first ring of a shape is wound so that its signed area is
positive in these coordinates (u east, v south); the others (courtyards) the other way: lib/services/vectormap.dart fills with the nonzero rule, so shapes that overlap
merge and a courtyard stays a hole. Map data (c) OpenStreetMap contributors, ODbL.

[region_osm.py]
What the builders of the shapes of the region (build_region_buildings.py, build_region_zones.py) share: the Mercator grid of the files (region_pack.py), the boxes of
the campuses, the rule that drops the points of a ring that are on the line of their neighbours, and the reader of an extract of OpenStreetMap (a .osm.pbf).

The extract is the one of BBBike for Stockholm (tools/fetch_region_pbf.py): about 58 MB, the area 17.54 to 18.56 E, 59.10 to 59.57 N, read in about 12 seconds with pyosmium
(`osmium`, in tools/.venv). Map data (c) OpenStreetMap contributors, ODbL.

[build_region_zones.py]
Builds the land use of the whole area of the 2D map, outside the boxes of the campuses, as ONE compact binary asset: housing, shops and offices, industry, school and hospital
grounds, car parks, pitches, pedestrian areas, platforms, sand and rock, wetland, railway land.

    tools/.venv/Scripts/python tools/build_region_zones.py [--cache tools/cache] [--pbf <extract>] [--out assets/data/vector_region_zones.bin] [--check | --verify | --stats]

The detail map of a campus has these as "zones" (tools/add_map_zones.py: the classes and the tags are the ones of ZONE_CLASSES there, and `zone_class` is used as it is) and the coarse
maps of the region and of the outer ring have none, so the map showed the land use as a rectangle of darker ground at the edge of every box, from zoom 14 (docs/OBRA_2D_REGION.md).
This is the data that closes it: the same classes, for the rest of the map, cut where the detail maps begin (the box of each campus and the margin of 0.001 degrees at which
tools/add_map_zones.py cuts its own), so that nothing is drawn twice and nothing is missing at the seam.

reads   <pbf>  (default <cache>/bbbike/Stockholm.osm.pbf)   the areas of OpenStreetMap of the extract of BBBike (tools/fetch_region_pbf.py): closed ways and multipolygons
writes  assets/data/vector_region_zones.bin                  read by lib/services/region_shapes.dart (class of each shape; courtyards as negative rings)

--check   builds again from the extract, in memory, and exits with 1 when the asset in the repository is not that
--verify  needs no extract: decodes the asset, and exits with 1 when it is damaged or breaks a rule that the app relies on
--stats   what is in the asset

The extract of BBBike covers 17.54 to 18.56 E and 59.10 to 59.57 N: the two strips of the frame of the outer map (west of 17.54, south of 59.10) have their land use from the second extract, the frame cut
out of the extract of Sweden (tools/strips.py, tools/cut_extract.py), read with the same rules over the rest of the frame (`--strips-pbf`; without the file the strips have none, as before).
The format is the one of tools/region_pack.py (shift 26: 0.3 m). Map data (c) OpenStreetMap contributors, ODbL.

[add_map_zones.py]
Adds the areas and lines the first map builders left out to the bundled campus maps: car parks, sports pitches,
playgrounds, pedestrian squares, railway platforms, land use (housing, shops and offices, industry, schools and
hospitals), sand, bare rock and wetland, and streams.

    tools/.venv/Scripts/python tools/add_map_zones.py [campus_id ...] [--refetch]

Per campus (Overpass, cached in tools/cache/zones_<id>.json) it writes into the campus map file
  "zones":   [[class, ring, ring...], ...]  areas, the first ring the outline, the others holes (see ZONE_CLASSES)
  "streams": [[lat, lng, ...], ...]         lines of streams and canals
Running it again replaces what it wrote before; nothing else in the file is touched.

    tools/.venv/Scripts/python tools/add_map_zones.py --pbf tools/cache/bbbike/Stockholm.osm.pbf [--check] [campus_id ...]

adds to the zones already in the files, with no request to Overpass, the classes 11 to 16 (farmland, allotments, golf course, quarry and landfill, airfield, other vegetation: the audit of 16 Nov 2026 found them drawn
nowhere) from the extract of BBBike (tools/fetch_region_pbf.py), cut at the edge of the box with the margin below; it replaces the zones of those classes that an earlier run wrote and leaves the others as they are.
(The land use of the rest of the map is tools/build_region_zones.py.) --check writes nothing and exits 1 when a file is not what the extract gives.

[build_region_paths.py]
Builds the paths of the whole area of the 2D and the 3D map, outside the boxes of the campuses, as ONE compact binary asset: the service roads and tracks, the footways, paths and cycleways, and the steps.

    tools/.venv/Scripts/python tools/build_region_paths.py [--cache tools/cache] [--pbf <extract>] [--out assets/data/vector_region_paths.bin] [--tolerance 2] [--check | --verify | --stats]

The detail map of a campus draws these (its `roads` of the classes 2 service or track, 3 footway, path, cycleway or bridleway, and 4 steps: tools/build_campuses.py) and the coarse maps of the region and of
the outer ring carry only the major roads and the streets, so outside the eight boxes 15,900 km of paths (1 % of them drawn: docs/AUDITORIA_2D_16NOV.md F04, docs/AUDITORIA_3D_SUELO_16NOV.md G3) were missing from both
maps: the park of Djurgarden, Haga, Tyresta, the housing areas, the cycle ways. This file carries them for the rest of the map, in the classes of the boxes, and both maps read it:
lib/services/region_shapes.dart (the 2D map, from zoom 15 as the paths of the boxes) and tools/map3d/ground3d.py (the 3D map, in the tiles of zoom 15 only).

reads   <pbf>  (default <cache>/bbbike/Stockholm.osm.pbf)   the highway ways of the extract of BBBike (tools/fetch_region_pbf.py)
writes  assets/data/vector_region_paths.bin               open lines with a class (tools/region_pack.py, flag bit 1); the class is the kind + 8 * the level:
                                                          kind 2 service road or track, 3 footway, path, cycleway or bridleway, 4 steps; level 0 the surface, 1 a bridge, 2 a tunnel
                                                          (tools/road_levels.py: a bridge is drawn over the railways and the roads, a tunnel is a ghost under them)

A path that lies in a box is drawn by the detail map of the box, which has the whole way (the query of the box takes the ways that touch it with all their nodes): so the file has nothing inside a box and
nothing within 5 m of what a detail map draws outside its box (the rest of a way that leaves it), and nothing is drawn twice. Lines are simplified to --tolerance metres (2 by default: the streets of the region are
simplified at 8) and a piece under 3 m is dropped. The extract of BBBike covers 17.54 to 18.56 E and 59.10 to 59.57 N: the two strips of the frame outside it have their paths from the second extract, the frame
cut out of the extract of Sweden (tools/strips.py, tools/cut_extract.py), read with the same rules (`--strips-pbf`; without the file the strips have none, as before).

--check   builds again from the extract, in memory, and exits with 1 when the asset in the repository is not that
--verify  needs no extract: decodes the asset, and exits with 1 when it is damaged or breaks a rule that the apps rely on
--stats   what is in the asset

The format is the one of tools/region_pack.py (shift 26: 0.3 m). Map data (c) OpenStreetMap contributors, ODbL.

[build_map_labels.py]
Builds the WORDS of the whole area of the 2D map as ONE compact binary asset: the names of streets, places, water, parks, stations, shops and named buildings.

    tools/.venv/Scripts/python tools/build_map_labels.py [--cache tools/cache] [--pbf <extract>] [--extras <file>] [--out assets/data/map_labels.bin] [--check | --verify | --stats]

Before it the 2D map had street names only inside the eight boxes of the campuses (one label for each street, from zoom 16) and no name of a place, a lake, a park, a station or a shop
anywhere (docs/OBRA_2D_TEXTO.md). This is the data that gives it its words: every label of the frame of the map (59.00 to 59.56 N, 17.30 to 18.45 E) with its class, the zoom from
which it may be written, its priority and its place. What is drawn at each zoom, and what is left out where two names meet, is decided in the app (lib/services/map_labels.dart).
The rules (which objects, which class, from which zoom) are tools/map_labels_osm.py; the format is the top of tools/map_labels_pack.py.

reads   <pbf>    (default <cache>/bbbike/Stockholm.osm.pbf)  the extract of BBBike (tools/fetch_region_pbf.py): every object that has a name
        <extras> (default tools/map_labels_extras.json)      what the extract lacks: the two strips of the frame that it does not cover (west of 17.535 E and south of 59.103 N) and the
                                                              multipolygons that it cannot assemble (Mälaren, the big islands...), fetched from the free Overpass servers by
                                                              tools/fetch_label_extras.py with the same rules; without it those words are missing
        assets/data/building_labels.json                     the names of buildings that the app already draws (the ones of the campuses): a named building is not written twice
writes  assets/data/map_labels.bin                           read by lib/services/map_labels.dart

--check   builds again from the sources, in memory, and exits with 1 when the asset in the repository is not that
--verify  needs no source: decodes the asset, and exits with 1 when it is damaged or breaks a rule that the app relies on
--stats   what is in the asset

The extract covers 17.54 to 18.56 E and 59.10 to 59.57 N. Map data (c) OpenStreetMap contributors, ODbL.

[map_labels_pack.py]
The binary container of the WORDS of the 2D map: the names of streets, places, water, parks, stations, shops and buildings of the whole area of the map
(assets/data/map_labels.bin), made by tools/build_map_labels.py and read by lib/services/map_labels.dart.

The 2D map is drawn by the app itself from OpenStreetMap shapes, and before this file it had text of five kinds only (street names inside the eight boxes of the campuses, names
of buildings, rooms, doors, pins): no suburb, island, lake, park or station had a name anywhere (docs/OBRA_2D_TEXTO.md). One label is a name, a place on the map and what it is.
The app keeps the file as it is and reads the labels of the cells that a part of the screen touches; what it draws at each zoom is decided there (class, size and collisions).
All integers are little-endian; `varint` is the LEB128 of Protocol Buffers. The coordinates are those of the Web Mercator map, 0..1 from the west and from the north, as whole
numbers of 2**-shift of the world (shift 23: 2.4 m at 59.3 N). Names are UTF-8 text as OpenStreetMap writes them (the language of the place: Swedish).

  header   56 bytes   magic "RLBL", version 1, shift, cell_shift, flags (0), then u32: col0, row0, cols, rows (the grid of cells: a cell is 2**cell_shift units, 512 units =
                      a tile of zoom 14), labels, used_cells, names, names_len, table_len, blocks_len; then u64 digest of everything that is drawn
  types    varint t, then t x 3 bytes (kind, minz, prio): the kinds of label that the file has, the commonest first. `kind` is class << 5 | sub (see CLASSES below); `minz` the first
           zoom level at which the label may be drawn, in quarters of a level (44 = zoom 11); `prio` what wins when two labels meet (0 first)
  names    names_len bytes: each distinct name, in the order of its first label in the file, as UTF-8 text followed by a line break (a name has no line break)
  table    used_cells x (varint: index of the cell minus that of the one before, varint: bytes of its block, varint: how many names are used for the first time in it);
           index = row * cols + col, both from the origin
  blocks   the cells in the order of the table. A block is: varint n, then n varints of the type of each label, n varints of its name, n varints of y and n varints of x. The labels
           of a block are the ones that are not streets by y, then x, and then the streets, the labels of a street name together. A name is zigzag(its number - the number that the next
           name used for the first time will have), so that a name that is new is 0 (that number starts at the count of names that the cells before have used); y and x are zigzag(the
           difference from the label before), the first from the top left corner of the cell

What a class is (`kind` >> 5) and its subclasses (`kind` & 31):
  0 street    0 motorway or trunk, 1 primary, 2 secondary, 3 tertiary, 4 residential, unclassified, living street, 5 pedestrian street. A label every 300 m of a named street
  1 place     0 city, 1 town, 2 suburb, 3 village, 4 neighbourhood, 5 quarter, 6 locality, 7 hamlet, 8 island, 9 islet, 10 square, 11 farm, 12 isolated dwelling, 13 archipelago
  2 water     0 lake, 1 bay, 2 strait, 3 river or canal, 4 pond, 5 sea, 6 other
  3 green     0 park, 1 nature reserve, 2 forest or wood, 3 cemetery, 4 garden, 5 golf course, 6 allotments, 7 wetland, 8 beach, 9 other green (heath, meadow, recreation ground)
  4 station   0 metro, 1 train, 2 tram or light rail, 3 ferry, 4 bus station
  5 poi       0 culture and tourism, 1 education, 2 health, 3 food and drink, 4 shops, 5 services, 6 sport and leisure, 7 lodging, 8 worship, 9 transport
  6 building  0 a named building

A cell holds the labels whose point lies in it. The digest is `digest_of`. Map data (c) OpenStreetMap contributors, ODbL.

[map_labels_osm.py]
The rules of the words of the 2D map: which objects of OpenStreetMap get a label, of what class, from which zoom and with what priority, and where the label goes.

What tools/build_map_labels.py (the extract of BBBike) and tools/fetch_label_extras.py (Overpass, for the two strips of the frame that the extract does not cover and for the multipolygons
that it cannot assemble) share. Nothing
here reads a file or the network: it takes the tags and the geometry of an object and gives back the candidates (`Cand`) that the builder then selects and writes
(tools/map_labels_pack.py is the format). The classes and their subclasses are listed at the top of that file. Map data (c) OpenStreetMap contributors, ODbL.

The rules in one place (the numbers were chosen by measuring the extract, docs/OBRA_2D_TEXTO.md sections 3 and 4):

  names       the `name` tag (else `name:sv`); a street with several names ("A;B") gives one label for each; a name shorter than 2 or longer than 60 letters, one with no letter,
              or one that is a sign and not a name is left out. Never a translation and never an invented name: an object with no name has no label
  streets     every named highway of the classes the map draws as streets (motorway to tertiary, residential, unclassified, living street, pedestrian). The ways of one name and one
              class are joined where they touch, and each joined line gets a label every 300 m (so that every point of a street is within 150 m of a label of its name), in the
              middle of each stretch, and, for the lines longer than 300 m, a second set between them every 150 m from zoom 17.5 and, on the roads of class secondary and above,
              a third every 75 m from zoom 18.75 (the app keeps two labels of one name 260 pixels apart, so these only show where a street fills the screen). A street is not
              written before it is long enough for its name: from the zoom at which its length on the screen is the width of its text
  places      city, town, suburb, village, neighbourhood, quarter, locality, hamlet, farm, isolated dwelling, square, island, islet, archipelago. Islands and islets by their area
  water       lakes, ponds, bays and straits (areas, by the area that lies inside the frame; the middle is the point of the polygon farthest from its edge, and a big water has one label for
              each 6 km of it), rivers and canals (every 1.5 km), and the nodes that OpenStreetMap puts a bay or a strait on
  green       parks, nature reserves and protected areas, gardens, forests and woods, cemeteries, golf courses, allotments, wetlands, beaches and the other greens, by their area
  stations    railway stations and halts, tram stops, public transport stations, ferry terminals and bus stations; one label for each name within 500 m (the entrances and the stop
              positions of a station are not labels)
  poi         a curated list of shops, services, food, culture, health, education, lodging and worship by class (POI, below), written from zoom 14.5 to 18.25 by how much a student
              looks for it; the long tail (one more hairdresser, any shop with a name that the list does not name) from zoom 18 or 18.25, when a screen is 120 m by 240 m
  buildings   a building with a name that is not a shop or a service already, by its footprint, from zoom 16

[fetch_label_extras.py]
Fetches, from the free Overpass servers, the words that the extract of BBBike does not have, and writes the labels of them (tools/map_labels_extras.json) for tools/build_map_labels.py.

    tools/.venv/Scripts/python tools/fetch_label_extras.py [--cache tools/cache/labels_extra] [--pbf <extract>] [--out tools/map_labels_extras.json] [--list] [--offline]
                                                          [--only relations|strips]

The extract (tools/fetch_region_pbf.py) covers 17.535 to 18.557 E and 59.103 to 59.571 N, and the frame of the 2D map is 17.30 to 18.45 E and 59.00 to 59.56 N, so it leaves out two strips:
the west one (17.30 to 17.535 E: Mälaren, Mörkö, Hölö, Mölnbo, Gnesta) and the south one (59.00 to 59.103 N: Järna, Tungelsta, Muskö, Sorunda). And its multipolygons that are cut by its
edge cannot be assembled (Mälaren, Värmdö, Södertörn, Ingarö, Ljusterö, Ornö, the lakes Fysingen, Vällingen and Flaten, bays, straits and golf courses: about 40 with a name), so they
have no area and no label. Two kinds of request make up for it:

  relations  each of those multipolygons, by its id, with its whole geometry (`relation(id); out geom;`): its label points are worked out as for any area
  strips     the named places, stations, shops, streets, waters, parks and buildings of the two strips, in cells of about 15 km, in three requests each (nodes, streets, areas)

Since 22 Nov 2026 the strips need no request when the second extract is there (tools/strips.py: the frame cut out of the extract of Sweden of Geofabrik, tools/cut_extract.py): tools/build_map_labels.py
reads them from it with the same rules, the labels that this script wrote for the strips are left out of the pack then, and only `--only relations` is still needed (the multipolygons that the extract
of BBBike cannot assemble). `--only strips` is the way to work without that file.
The rules are the ones of tools/map_labels_osm.py and tools/build_map_labels.py: this script only brings the objects in and hands them to the same functions, and
keeps what lies outside the extract (a strip) or what the extract lacks (a relation). Gentle with the servers: ONE request at a time, 8 seconds between requests, a longer wait after an
error, a User-Agent that names a study project and nothing else; each answer is kept in the cache folder and a rerun goes on from there. About 70 requests in all, half an hour to an hour
when the servers are not busy. Nothing is sent but the query. Map data (c) OpenStreetMap contributors, ODbL.

--list     prints the requests and which are already in the cache
--offline  works the labels out from the cache only, with no request (the day the servers are down)

[bike_graph_codec.py]
The file format of the bike ways bundled in the app (assets/data/bike_graph.bin): the reference encoder and decoder.

The app reads it with lib/services/bikegraph.dart (BikeGraph.decode); both are tied to this file by the fixtures of test/fixtures
(bike_graph_mini*.bin and .json, made by `python tools/bike_graph_codec.py --fixtures`). docs/RIDING_MODES.md has the meaning of every
number. All integers are little endian; a "varint" is an unsigned LEB128; "zigzag" maps a signed integer n to (n << 1) ^ (n >> 63)
and then writes it as a varint. Coordinates are integer micro-degrees (degrees times 1,000,000).

    header   u32 magic 0x47424D43 ("CMBG" as bytes 43 4D 42 47) | u16 version = 1 | u16 flags (bit 0: every edge has a grade)
             | u32 nodeCount | u32 edgeCount | u32 shapeCount | u16 boxCount | u16 reserved = 0
             | boxCount x 4 x i32: south, west, north, east of each area the file covers
    sections four of them, each a u32 byte length followed by that many bytes:
      NODES      nodeCount x [zigzag dLat, zigzag dLng]            deltas from the previous node (the first from 0, 0)
      NODEFLAGS  nodeCount x u8                                    bit 0 traffic lights, bit 1 marked crossing, bit 2 barrier
      EDGES      edgeCount x [zigzag (a - previous a), zigzag (b - a), varint length in decimetres, u8 class, u8 attrs,
                              (i8 grade, only when the header says so), varint shape points of this edge]
      SHAPE      shapeCount x [zigzag dLat, zigzag dLng]           the points between the ends of each edge, in order, each as
                                                                   the delta from the one before it (the first from node a)
    attrs      bits 0-2 surface class, bits 3-4 direction (0 both ways, 1 only a to b, 2 only b to a), bit 5 lit, bits 6-7 zero.
    class      0 cycleway, 1 shared path, 2 street with a bike lane or track, 3 quiet street, 4 street, 5 busy road,
               6 main road, 7 push (walk with the machine by hand)
    surface    0 unknown, 1 smooth, 2 paving stones or compacted, 3 gravel, 4 cobblestones, 5 unpaved, 6 and 7 not used
    grade      percent of climb going from a to b (a descent is negative), clamped to -30..30; going from b to a it is the opposite

[bike_classify.py]
The rules of docs/RIDING_MODES.md, section 4.2, as pure functions: what a rider can do on an OpenStreetMap way or node.

    classify_way(tags)  -> [Edge, ...]   the directed edges a way gives a bike, in the direction of the way (its first node to its last):
                                         [] when a bike may not use it, one edge when both ways are alike, two when they differ
                                         (a one-way street, a lane on one side only)
    exclusion(tags)     -> str | None    why a bike may not use the way (None when it may)
    node_flags(tags)    -> int           bit 0 traffic lights, bit 1 marked crossing, bit 2 barrier (the rider gets off)
    surface_class(tags) -> int           0 unknown ... 5 unpaved, as the file

An Edge is (cls, surf, dir, lit, incline): the class and surface numbers of tools/bike_graph_codec.py, dir BOTH / FORWARD (first node
to last) / BACKWARD, lit a bool, incline the percent of climb going from the first node to the last when OpenStreetMap says it, else None.
Nothing here reads a file or the network: the builder (build_bike_graph.py) calls these for every way and node, and
test_bike_classify.py pins every rule.

Where the spec leaves a gap this file decides, and says so next to the decision (the report lists them):
  * a street with a lane on a road above tertiary (secondary, primary, trunk) is not class 2; a separated track is, a painted lane
    takes the road one class down, a shared-lane marking changes nothing;
  * a tertiary street with no speed given is class 4, a tertiary street at 30 or less too (not a quiet street);
  * a cycle street (bicycle_road=yes, cyclestreet=yes) is class 2;
  * a path or track that is only for people on foot (foot=designated, no word about bicycles) or where bicycles are "discouraged" is
    class 7, like a footway;
  * a primary or trunk road with 90 km/h or more and no word that bicycles may use it is not used at all (it is as good as a motorway).
(c) OpenStreetMap contributors, ODbL.

[build_bike_graph.py]
Build the bike network of the app (assets/data/bike_graph.bin, format of tools/bike_graph_codec.py) from OpenStreetMap (ODbL).

    tools/.venv/Scripts/python tools/fetch_bike_raw.py        # once: the raw data (cached in tools/cache/bike, resumable)
    tools/.venv/Scripts/python tools/build_bike_graph.py      # writes assets/data/bike_graph.bin and prints the figures
    tools/.venv/Scripts/python tools/bike_graph_check.py      # the validation of docs/RIDING_MODES.md section 4.6

Sources: tools/cache/region_roads.json (the streets of greater Stockholm) and the files of fetch_bike_raw.py (cycleways, paths,
footways, tracks, service roads; and every way of Sodertalje and Haninge), plus the nodes with tags (traffic lights, crossings,
barriers). The rules of what a bike may do on a way are in bike_classify.py; the areas and the zones in bike_areas.py.

What it does, in order:
  1. every way is classified (bike_classify.classify_way); a way a bike may not use is left out; ways are cut at the edge of the areas
  2. the graph is made of the OpenStreetMap nodes the ways SHARE (an intersection), the ends of ways and the nodes with a flag: a way is
     cut there into units; nothing connects two ways that do not share a node, except the short gaps repaired in step 4
  3. a stretch of a footway or a path that lies on a flight of steps (the steps drawn twice) is dropped; contraction: a node where exactly
     two units of the same attributes meet, with no flag, is not a node (the shape keeps the geometry)
  4. repairs of the data: a way that ends within a few metres of another way it was meant to join gets a link (a mapper's gap; a cycleway
     that stops up to 12 m short, a shared path 8 m, gets a link to walk); never where steps are, across a building (campus maps) or a
     railway; a loop is cut in two, two ways on the same nodes become one. With --near-links (not in the shipped file) also two nodes up
     to 12 m apart, one of them on a cycle way and the other on a street or a cycle way, that no way of 150 m or less joins in both
     directions (link_near_nodes)
  5. components under 40 nodes are dropped unless a campus is in them
  6. one-way pockets (a few ways a rider can enter and not leave, or the other way round) get the cheapest ways out and in as a second
     edge of class 7 against the traffic, never on a busy or a main road, a bridge or a tunnel
  7. size (4.3): when the file is over the limit, outside the zones of the campuses the unneeded service ways, tracks and pushed
     footways are dropped, in this order, each step measured (then the unmarked paths, dead ends of pushed footways and a coarser
     shape if it is still too big); nothing that connects two bike ways is dropped (a way goes only when its ends stay joined by other
     ways that are not much longer)
  8. nodes are ordered along a Hilbert curve (so the deltas of the file are small), shapes are simplified to 3 m, and the file is written

The numbers are printed and kept in tools/cache/bike/build_report.json (docs/BIKE_GRAPH_REPORT.md has the last run).
