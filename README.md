# Task C: සම්පූර්ණ මාර්ගෝපදේශය (Ultimate Guide)

අන්න එහෙම එන්න! කිසිම අවුලක් නෑ, මම මුල ඉඳන් අගටම එකම වැරැද්දක්වත් නැතුව, අර අලුත් ක්‍රමත් ඔක්කොම එකතු කරලා (ලකුණු 30න් 30ම ගන්න පුළුවන් විදිහට) Task C එක කරන සම්පූර්ණ පියවර ටික මෙන්න මේ ෆයිල් එකේ ලිව්වා. පරණ සේරම අමතක කරලා මේක පිළිවෙලට කරගෙන යන්න.

---

## 🟢 Step 1: Database එක හැදීම (PostgreSQL/PostGIS)
1. **pgAdmin 4** මෘදුකාංගය Open කරලා ඔයාගේ Password එක දීලා ඇතුලට යන්න.
2. වම් පැත්තේ ලිස්ට් එකෙන් `Servers -> PostgreSQL -> Databases` උඩ Right-click කරලා `Create -> Database...` යන්න.
3. Database එකේ නමට **SL_BIA_Aerial_Info** කියලා දීලා Save කරන්න.
4. හැදුණු Database එක උඩ Right-click කරලා **Query Tool** යන්න.
5. එතන `CREATE EXTENSION postgis;` කියලා ටයිප් කරලා උඩ තියෙන ත්‍රිකෝණය (Run/Execute) ඔබන්න. (දැන් Database එක ලෑස්තියි).
6. QGIS Open කරගන්න. වම් පැත්තේ Browser පැනල් එකේ PostGIS උඩ Right-click කරලා **New Connection** යන්න.
7. `Name: BIA_Database`, `Host: localhost`, `Port: 5432`, `Database: SL_BIA_Aerial_Info` විදිහට දීලා, Basic යටතේ ඔයාගේ Username (postgres) සහ Password එක දීලා OK කරන්න.
8. දැන් ඔයාගේ ෆෝල්ඩර් එකේ තියෙන ෂේප් ෆයිල්ස් 5ම (`Admin Regions.shp`, `Air Force Base Katunayake.shp`, `Air Force Base Region.shp`, `Airport Places New.shp`, `Airport Places.shp`) QGIS එකේ Layers පැනල් එකට Drag කරලා දාන්න.
9. ඊට පස්සේ ඒ ලේයර්ස් 5ම වම් පැත්තේ Browser එකේ තියෙන ඔයාගේ අලුත් PostGIS කනෙක්ෂන් එකේ `public` කියන එක උඩට අදින්න (Drag and Drop). *Import was successful* කියලා වැටෙයි.

---

## 🟢 Step 2: Georeferencing සහ ගොඩනැගිලි ඇඳීම (Digitizing)
1. ෆෝල්ඩර් එකේ තියෙන `Bandaranayake Airport Areal Latest3_1_modified.tif` ෆයිල් එක QGIS එකට Drag කරන්න.
2. QGIS තිරයේ යටම දකුණු කෙළවරේ තියෙන EPSG ලෝක ගෝලය ඔබලා Search එකේ `5234` ගහලා **Kandawala** තෝරලා OK කරන්න.
3. Layers පැනල් එකේ අර .tif ෆයිල් එක උඩ Right-click කරලා `Layer CRS -> Set Layer CRS` ගිහින් එතනිනුත් `5234 - Kandawala` තෝරන්න. *(මේකෙ Screenshot එකක් ගන්න)*.
4. Menu එකේ `Layer -> Create Layer -> New Shapefile Layer` යන්න.
5. `File name: Digitized_Buildings`, `Geometry type: Polygon`, `CRS: EPSG:5234 - Kandawala` තෝරන්න.
6. යටින් New Field යටතේ මේ 3 Add කරන්න: 
   - `name` (Text data)
   - `type` (Text data)
   - `size` (Decimal number)
7. OK කරන්න. දැන් Layers ලිස්ට් එකට ආපු **Digitized_Buildings** උඩ Click කරලා කහ පාට පැන්සල (Toggle Editing) ඔබන්න.
8. Add Polygon Feature ඔබලා එයාපෝට් එකේ තියෙන ප්‍රධාන ගොඩනැගිලි 15ක් වටේට අඳින්න (Terminal, Control Tower, Runway Center Point, Military Billets වගේ දේවල්).
9. එකක් ඇඳලා ඉවරවෙලා Right Click කරාම එන Box එකේ id එක, name එක, type එක දෙන්න. හැබැයි size එක හිස්ව තියන්න! *(ඔයාට ලේසි වෙන්න මෙන්න ඒ නම් 15 සහ ඒවගේ Type එක)*:
   - `Smoke Room Practice Area` -> **Training Area**
   - `Rescue Traning Tower` -> **Training Facility**
   - `Wo & SNCO Mess` -> **Military Building**
   - `C44 Billet` -> **Military Building**
   - `Electronic and Telecommunication` -> **Military Building**
   - `Command Agro SLAF Base Katunayake` -> **Military Area**
   - `Air Force Ground Katunayake` -> **Open Ground**
   - `BIA Control Tower` -> **Aviation Facility**
   - `C242 Airman Bilate` -> **Military Building**
   - `Ceylon Petroleum Corporation Aviation` -> **Fuel Facility**
   - `Air Cargo Village Entrance` -> **Commercial Building**
   - `Departure Terminal` -> **Terminal Building**
   - `Arrival Terminal` -> **Terminal Building**
   - `Terminal Car Park` -> **Parking Area**
   - `Runaway Center Point` -> **Runway Infrastructure**
10. ගොඩනැගිලි ටික ඇඳලා ඉවර වුණාම, Digitized_Buildings උඩ Right-click කරලා **Open Attribute Table** යන්න.
11. ඒකෙ Field Calculator 🧮 ඔබලා, *Update existing field* ටික් කරලා, drop-down එකෙන් `size` තෝරලා, Expression එකට `$area` කියලා ගහලා OK කරන්න. (දැන් වර්ගඵල ටික ඔටෝ පිරෙයි). පැන්සල ඔබලා Save කරන්න.
12. මේ **Digitized_Buildings** ලේයර් එකත් අර `PostGIS -> public` එකට Drag කරලා දාන්න.

---

## 🟢 Step 3: රේඩාර් තියන තැන් හරියටම හොයාගැනීම (QGIS + Google Earth)

**SMR එක හොයමු:**
1. මුලින්ම `Airport Places New` ලේයර් එක උඩ Right-click කරලා `Open Attribute Table` යන්න.
2. ඒකෙ තියෙන **BIA Control Tower** පේළියේ වම් කෙළවරේ තියෙන අංකය උඩ Click කරලා ඒ පේළිය තෝරන්න (නිල් පාට කරන්න).
3. දැන් `Vector -> Geoprocessing Tools -> Buffer` යන්න. Input layer එකට `Airport Places New [EPSG:5234]` දීලා, ඊට යටින් තියෙන **Selected features only** කියන පොඩි කොටුවට අනිවාර්යයෙන්ම ටික් (✔) එකක් දාන්න. Distance එක `300` දීලා Run කරන්න.
4. ඊළඟට, ආයෙත් Attribute Table එකට ගිහින් **Runaway Center Point** කියන පේළිය තෝරන්න (නිල් පාට කරන්න). ආයෙත් Buffer ගිහින්, **Selected features only** ටික් එක දාලා, Distance එක `200` දීලා Run කරන්න.
5. ඒ රවුම් දෙක කැපෙන හරිය තමයි SMR එක ගහන්න ඕන හරියම තැන.

**PSR/SSR හොයමු:**
1. අර විදිහටම **Runaway Center Point** එක තෝරලා තියෙද්දීම (Selected features only ටික් එක දාලා), ආයෙත් Buffer ගිහින් Distance එක `2000` (2km) දීලා Run කරන්න.
2. ආයෙත් ඒ විදිහටම Distance එක `3000` (3km) දීලා Run කරන්න.
3. ඒ රවුම් දෙක අතර කලාපය Air Force Base එක ඇතුළට අහුවෙන හරිය තමයි PSR සහ SSR එක ගහන්න ඕන තැන.

**Google Earth එකෙන් KML හැදීම (අනිවාර්යයි):**
1. Google Earth Pro මෘදුකාංගය Open කරගන්න. 
2. අර අපි QGIS එකේ රවුම් ගහලා හොයාගත්ත හරියම තැන් 3, Google Earth එකෙන් බලලා ඒ තැන් 3ට කහ පාට පින් 3ක් ගහන්න. ඒවට **PSR Radar**, **SSR Radar**, **SMR Radar** කියලා නම් දෙන්න.
3. වම් පැත්තේ My Places යටතේ ඒ පින් 3ම තෝරලා Right-click -> Save Place As... ගිහින් **Radar_Locations.kml** කියලා සේව් කරන්න. (Type එක .kml වෙන්න ඕනේ).

**QGIS එකට KML එක දැමීම:**
1. ඒ `Radar_Locations.kml` එක QGIS එකට අදින්න.
2. ඒක උඩ Right-click කරලා `Export -> Save Features As...` යන්න.
3. Format: `ESRI Shapefile`, Name: `Radar_Projected`, CRS: `EPSG:5234 - Kandawala` අනිවාර්යයෙන්ම දීලා OK කරන්න.
4. පරණ KML එක අයින් කරන්න. මේ **Radar_Projected** එකත් අර `PostGIS -> public` එකට දාන්න.

---

## 🟢 Step 4: Geoprocessing (විශ්ලේෂණය) සහ ගණනය කිරීම්
1. `Vector -> Geoprocessing Tools -> Buffer` යන්න.
2. Input එකට අර අලුත් Radar_Projected ලේයර් එක දීලා, Distance එක `500` දීලා Run කරන්න. 
3. මේ හැදෙන අලුත් ලේයර් එක තමයි "Suitability Zones" (මේකත් `PostGIS -> public` එකට දාන්න).
4. `Vector -> Geoprocessing Tools -> Intersection` යන්න.
5. Input එකට Digitized_Buildings දීලා, Overlay එකට අර අලුතින් ආපු Buffered ලේයර් එක දීලා Run කරන්න.
6. දැන් එන Intersection ලේයර් එක උඩ Right-click කරලා `Export -> Save Features As...` ගිහින්, Name එකට **Intersected_Buildings** කියලා දීලා Save කරගන්න. 
7. අලුතින් හැදුණු Intersected_Buildings ලේයර් එක උඩ Right-click කරලා Open Attribute Table ගිහින්, Field Calculator 🧮 ඔබලා size field එක Update කරන්න (Expression: `$area`). Save කරන්න. (මේ ලේයර් එකත් PostGIS එකට දාන්න).
7. ගණන් ගැනීම: `Vector -> Analysis Tools -> Basic Statistics for Fields` ගිහින්, Intersection ලේයර් එකේ size field එක දීලා Run කරන්න.
8. එන උත්තරේ **Count** එක (Total Buildings) සහ **Sum** එක (Occupied Area) කොලේක ලියාගන්න.
9. Available Area එක හොයන්න: `(3.14159 × 500 × 500 × 3)` කියන එකෙන් අර Sum එක අඩු කරන්න. 

---

## 🟢 Step 5: සිතියම Print කිරීම (Layout)
1. `Project -> New Print Layout` යන්න.
2. Add Map ඔබලා සිතියම අඳින්න.
3. Add Label ගිහින් මාතෘකාව (**Radar Deployment Suitability Map - BIA**) දාන්න.
4. Add Legend, Add North Arrow, Add Scale Bar දාගන්න.
5. `Layout -> Export as Image` ගිහින් **Final_Radar_Map.png** කියලා සේව් කරගන්න!

---
> මෙන්න මේ ටික මේ විදිහටම කළාම ඔයාට ලකුණු 30න් එකක්වත් කපන්න බෑ මචං. පටන් ගමු!
