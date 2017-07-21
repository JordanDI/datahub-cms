## Global reference files & control paramteres

#### `global.channel` table definition
```
 Column |       Type        | Modifiers
--------+-------------------+-----------
 id     | character varying | not null
 name   | character varying | not null
Indexes:
    "channel_pkey" PRIMARY KEY, btree (id)
    "channel_id_key" UNIQUE CONSTRAINT, btree (id)
    "channel_name_key" UNIQUE CONSTRAINT, btree (name)
Referenced by:
    TABLE "oda" CONSTRAINT "unbundling_aid_oda_channel_fkey" FOREIGN KEY (channel) REFERENCES channel(id) ON UPDATE CASCADE ON DELETE RESTRICT
```
#### `global.colour` table definition
```
 Column |       Type        | Modifiers
--------+-------------------+-----------
 id     | character varying | not null
 value  | character varying | not null
Indexes:
    "colour_pkey" PRIMARY KEY, btree (id)
    "colour_id_key" UNIQUE CONSTRAINT, btree (id)
    "colour_value_key" UNIQUE CONSTRAINT, btree (value)
Check constraints:
    "valid_value" CHECK (value::text ~* '^#[0-9A-F]{6}$'::text)
    "valid_value_length" CHECK (length(value::text) = 7)
Referenced by:
    TABLE "oda_bundle" CONSTRAINT "oda_bundle_colour_fkey" FOREIGN KEY (colour) REFERENCES colour(id) ON UPDATE CASCADE ON DELETE RESTRICT
    TABLE "region" CONSTRAINT "region_colour_fkey" FOREIGN KEY (colour) REFERENCES colour(id) ON UPDATE CASCADE ON DELETE RESTRICT
    TABLE "sector" CONSTRAINT "sector_colour_fkey" FOREIGN KEY (colour) REFERENCES colour(id) ON UPDATE CASCADE ON DELETE RESTRICT
```
#### `global.currency` table definition
```
Column |       Type        | Modifiers
--------+-------------------+-----------
 id     | character(2)      | not null
 name   | character varying | not null
 code   | character(3)      | not null
Indexes:
    "currency_pkey" PRIMARY KEY, btree (id)
    "currency_id_key" UNIQUE CONSTRAINT, btree (id)
Foreign-key constraints:
    "currency_id_fkey" FOREIGN KEY (id) REFERENCES iso_3166_1(alpha_2_code) ON UPDATE CASCADE ON DELETE RESTRICT
```
#### `global.di_id` table definition
```
 Column |       Type        | Modifiers
--------+-------------------+-----------
 di_id  | character varying | not null
 name   | character varying | not null
Indexes:
    "di_id_pkey" PRIMARY KEY, btree (di_id)
    "di_id_di_id_key" UNIQUE CONSTRAINT, btree (di_id)
    "di_id_name_key" UNIQUE CONSTRAINT, btree (name)
Referenced by:
    TABLE "di_id_to_iso_3166_1_map" CONSTRAINT "di_id_to_iso_3166_1_map_di_id_fkey" FOREIGN KEY (di_id) REFERENCES di_id(di_id) ON UPDATE CASCADE ON DELETE RESTRICT
    TABLE "oecd_donor_to_di_id_map" CONSTRAINT "oecd_donor_to_di_id_map_di_id_fkey" FOREIGN KEY (di_id) REFERENCES di_id(di_id) ON UPDATE CASCADE ON DELETE RESTRICT
    TABLE "oda" CONSTRAINT "unbundling_aid_oda_from_di_id_fkey" FOREIGN KEY (from_di_id) REFERENCES di_id(di_id) ON UPDATE CASCADE ON DELETE RESTRICT
    TABLE "oda" CONSTRAINT "unbundling_aid_oda_to_di_id_fkey" FOREIGN KEY (to_di_id) REFERENCES di_id(di_id) ON UPDATE CASCADE ON DELETE RESTRICT
```
#### `global.di_id_to_iso_3166_1_map` table definition
```
    Column    |       Type        | Modifiers
--------------+-------------------+-----------
 di_id        | character varying | not null
 alpha_2_code | character(2)      |
Indexes:
    "di_id_to_iso_3166_1_map_pkey" PRIMARY KEY, btree (di_id)
    "di_id_to_iso_3166_1_map_di_id_key" UNIQUE CONSTRAINT, btree (di_id)
    "di_id_to_iso_3166_1_map_alpha_2_code_key" UNIQUE CONSTRAINT, btree (alpha_2_code)
Foreign-key constraints:
    "di_id_to_iso_3166_1_map_alpha_2_code_fkey" FOREIGN KEY (alpha_2_code) REFERENCES iso_3166_1(alpha_2_code) ON UPDATE CASCADE ON DELETE RESTRICT
    "di_id_to_iso_3166_1_map_di_id_fkey" FOREIGN KEY (di_id) REFERENCES di_id(di_id) ON UPDATE CASCADE ON DELETE RESTRICT
```
#### `global.iso_3166_1` table definition
```
    Column    |       Type        | Modifiers
--------------+-------------------+-----------
 alpha_2_code | character(2)      | not null
 country_name | character varying | not null
 alpha_3_code | character(3)      |
 numeric_code | smallint          |
Indexes:
    "iso_3166_1_pkey" PRIMARY KEY, btree (alpha_2_code)
    "iso_3166_1_alpha_2_code_key" UNIQUE CONSTRAINT, btree (alpha_2_code)
    "iso_3166_1_country_name_key" UNIQUE CONSTRAINT, btree (country_name)
    "iso_3166_1_alpha_3_code_key" UNIQUE CONSTRAINT, btree (alpha_3_code)
    "iso_3166_1_numeric_code_key" UNIQUE CONSTRAINT, btree (numeric_code)
Check constraints:
    "valid_alpha_2_code" CHECK (character_length(alpha_2_code) = 2)
    "valid_alpha_3_code" CHECK (character_length(alpha_3_code) = 3)
Referenced by:
    TABLE "currency" CONSTRAINT "currency_id_fkey" FOREIGN KEY (id) REFERENCES iso_3166_1(alpha_2_code) ON UPDATE CASCADE ON DELETE RESTRICT
    TABLE "di_id_to_iso_3166_1_map" CONSTRAINT "di_id_to_iso_3166_1_map_alpha_2_code_fkey" FOREIGN KEY (alpha_2_code) REFERENCES iso_3166_1(alpha_2_code) ON UPDATE CASCADE ON DELETE RESTRICT
    TABLE "oecd_donor_to_iso_3166_1_map" CONSTRAINT "oecd_donor_to_iso_3166_1_map_alpha_2_code_fkey" FOREIGN KEY (alpha_2_code) REFERENCES iso_3166_1(alpha_2_code) ON UPDATE CASCADE ON DELETE RESTRICT
```
#### `global.oda_bundle` table definition
```
 Column |       Type        | Modifiers
--------+-------------------+-----------
 id     | character varying | not null
 name   | character varying | not null
 colour | character varying | not null
Indexes:
    "oda_bundle_pkey" PRIMARY KEY, btree (id)
    "oda_bundle_id_key" UNIQUE CONSTRAINT, btree (id)
    "oda_bundle_name_key" UNIQUE CONSTRAINT, btree (name)
    "oda_bundle_colour_key" UNIQUE CONSTRAINT, btree (colour)
Foreign-key constraints:
    "oda_bundle_colour_fkey" FOREIGN KEY (colour) REFERENCES colour(id) ON UPDATE CASCADE ON DELETE RESTRICT
Referenced by:
    TABLE "oda" CONSTRAINT "unbundling_aid_oda_bundle_fkey" FOREIGN KEY (bundle) REFERENCES oda_bundle(id) ON UPDATE CASCADE ON DELETE RESTRICT
```
#### `global.oecd_donor` table definition
```
 Column |       Type        | Modifiers
--------+-------------------+-----------
 code   | smallint          | not null
 name   | character varying | not null
 type   | character varying |
Indexes:
    "oecd_donor_pkey" PRIMARY KEY, btree (code)
    "oecd_donor_code_key" UNIQUE CONSTRAINT, btree (code)
    "oecd_donor_name_key" UNIQUE CONSTRAINT, btree (name)
Foreign-key constraints:
    "oecd_donor_type_fkey" FOREIGN KEY (type) REFERENCES oecd_donor_type(id) ON UPDATE CASCADE ON DELETE RESTRICT
Referenced by:
    TABLE "oecd_donor_to_di_id_map" CONSTRAINT "oecd_donor_to_di_id_map_donor_code_fkey" FOREIGN KEY (donor_code) REFERENCES oecd_donor(code) ON UPDATE CASCADE ON DELETE RESTRICT
    TABLE "oecd_donor_to_iso_3166_1_map" CONSTRAINT "oecd_donor_to_iso_3166_1_map_donor_code_fkey" FOREIGN KEY (donor_code) REFERENCES oecd_donor(code) ON UPDATE CASCADE ON DELETE RESTRICT
```
#### `global.oecd_donor_to_di_id_map` table definition
```
   Column   |       Type        | Modifiers
------------+-------------------+-----------
 donor_code | smallint          | not null
 di_id      | character varying |
Indexes:
    "oecd_donor_to_di_id_map_pkey" PRIMARY KEY, btree (donor_code)
    "oecd_donor_to_di_id_map_donor_code_key" UNIQUE CONSTRAINT, btree (donor_code)
    "oecd_donor_to_di_id_map_di_id_key" UNIQUE CONSTRAINT, btree (di_id)
Foreign-key constraints:
    "oecd_donor_to_di_id_map_di_id_fkey" FOREIGN KEY (di_id) REFERENCES di_id(di_id) ON UPDATE CASCADE ON DELETE RESTRICT
    "oecd_donor_to_di_id_map_donor_code_fkey" FOREIGN KEY (donor_code) REFERENCES oecd_donor(code) ON UPDATE CASCADE ON DELETE RESTRICT
```
#### `global.oecd_donor_to_iso_3166_1_map` table definition
```
    Column    |     Type     | Modifiers
--------------+--------------+-----------
 donor_code   | smallint     | not null
 alpha_2_code | character(2) |
Indexes:
    "oecd_donor_to_iso_3166_1_map_pkey" PRIMARY KEY, btree (donor_code)
    "oecd_donor_to_iso_3166_1_map_donor_code_key" UNIQUE CONSTRAINT, btree (donor_code)
    "oecd_donor_to_iso_3166_1_map_alpha_2_code_key" UNIQUE CONSTRAINT, btree (alpha_2_code)
Foreign-key constraints:
    "oecd_donor_to_iso_3166_1_map_alpha_2_code_fkey" FOREIGN KEY (alpha_2_code) REFERENCES iso_3166_1(alpha_2_code) ON UPDATE CASCADE ON DELETE RESTRICT
    "oecd_donor_to_iso_3166_1_map_donor_code_fkey" FOREIGN KEY (donor_code) REFERENCES oecd_donor(code) ON UPDATE CASCADE ON DELETE RESTRICT
```
#### `global.oecd_donor_type` table definition
```
 Column |       Type        | Modifiers
--------+-------------------+-----------
 id     | character varying | not null
Indexes:
    "oecd_donor_type_pkey" PRIMARY KEY, btree (id)
    "oecd_donor_type_id_key" UNIQUE CONSTRAINT, btree (id)
Referenced by:
    TABLE "oecd_donor" CONSTRAINT "oecd_donor_type_fkey" FOREIGN KEY (type) REFERENCES oecd_donor_type(id) ON UPDATE CASCADE ON DELETE RESTRICT
```
#### `global.region` table definition
```
 Column |       Type        | Modifiers
--------+-------------------+-----------
 id     | character varying | not null
 name   | character varying | not null
 colour | character varying | not null
Indexes:
    "region_pkey" PRIMARY KEY, btree (id)
    "region_id_key" UNIQUE CONSTRAINT, btree (id)
    "region_name_key" UNIQUE CONSTRAINT, btree (name)
    "region_colour_key" UNIQUE CONSTRAINT, btree (colour)
Foreign-key constraints:
    "region_colour_fkey" FOREIGN KEY (colour) REFERENCES colour(id) ON UPDATE CASCADE ON DELETE RESTRICT
```
#### `global.sector` table definition
```
 Column |       Type        | Modifiers
--------+-------------------+-----------
 id     | character varying | not null
 name   | character varying | not null
 colour | character varying | not null
Indexes:
    "sector_pkey" PRIMARY KEY, btree (id)
    "sector_id_key" UNIQUE CONSTRAINT, btree (id)
    "sector_name_key" UNIQUE CONSTRAINT, btree (name)
    "sector_colour_key" UNIQUE CONSTRAINT, btree (colour)
Foreign-key constraints:
    "sector_colour_fkey" FOREIGN KEY (colour) REFERENCES colour(id) ON UPDATE CASCADE ON DELETE RESTRICT
Referenced by:
    TABLE "oda" CONSTRAINT "unbundling_aid_oda_sector_fkey" FOREIGN KEY (sector) REFERENCES sector(id) ON UPDATE CASCADE ON DELETE RESTRICT
```
## Notes

#### `sector.csv`

id|name|colour
---|---|---
other-sectors-grouped|Other sectors (grouped)|grey-dark

The [`other-sectors-grouped`](https://github.com/devinit/datahub-cms/blob/master/global/sector.csv#L16) sector is used with the [multilateral profiles](http://data.devinit.org/#!/multilaterals). It is based on a business rule that groups sectors together. The logic is: get top 5 sectors for an organization and group all the other sectors into one category.

---

#### `oda_bundle.csv`

UPDATE global.oda_bundle
SET id = 'gis_nngos' WHERE id = ['gpgs_nngos'](https://github.com/devinit/datahub-cms/blob/master/global/bundle.csv#L5);

"Global public goods & NNGOs" was changed to "Global initiatives & NNGOs".

---

#### `entity.csv` which will possibly be merged with `di_id.csv`

We use column `name` on a country's digital profile page.
