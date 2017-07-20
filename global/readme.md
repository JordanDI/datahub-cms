## Global reference files & control paramteres

#### `global.colour` table definition
```
 Column |       Type        | Modifiers 
--------+-------------------+-----------
 id     | character varying | not null
 value  | character varying | not null
Indexes:
    "colour_pkey" PRIMARY KEY, btree (id)
    "colour_value_key" UNIQUE CONSTRAINT, btree (value)
Check constraints:
    "valid_value" CHECK (value::text ~* '^#[0-9A-F]{6}$'::text)
    "valid_value_length" CHECK (length(value::text) = 7)
Referenced by:
    TABLE "oda_bundle" CONSTRAINT "oda_bundle_colour_fkey" FOREIGN KEY (colour) REFERENCES colour(id) ON UPDATE CASCADE ON DELETE RESTRICT
    TABLE "region" CONSTRAINT "region_colour_fkey" FOREIGN KEY (colour) REFERENCES colour(id) ON UPDATE CASCADE ON DELETE RESTRICT
    TABLE "sector" CONSTRAINT "sector_colour_fkey" FOREIGN KEY (colour) REFERENCES colour(id) ON UPDATE CASCADE ON DELETE RESTRICT
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
    "region_colour_key" UNIQUE CONSTRAINT, btree (colour)
    "region_name_key" UNIQUE CONSTRAINT, btree (name)
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
    "sector_colour_key" UNIQUE CONSTRAINT, btree (colour)
    "sector_id_key" UNIQUE CONSTRAINT, btree (id)
    "sector_name_key" UNIQUE CONSTRAINT, btree (name)
Foreign-key constraints:
    "sector_colour_fkey" FOREIGN KEY (colour) REFERENCES colour(id) ON UPDATE CASCADE ON DELETE RESTRICT
Referenced by:
    TABLE "oda" CONSTRAINT "unbundling_aid_oda_sector_fkey" FOREIGN KEY (sector) REFERENCES sector(id) ON UPDATE CASCADE ON DELETE RESTRICT
```

## Notes

#### `sector.csv`

id|name|colour
---|---|---
other_sectors_grouped|Other sectors (grouped)|grey_mid

The [`other-sectors-grouped`](https://github.com/devinit/datahub-cms/blob/master/global/sector.csv#L16) sector is used with the [multilateral profiles](http://data.devinit.org/#!/multilaterals). It is based on a business rule that groups sectors together. The logic is: get top 5 sectors for an organization and group all the other sectors into one category.

---

#### `oda_bundle.csv`

UPDATE global.oda_bundle
SET id = 'gis_nngos' WHERE id = ['gpgs_nngos'](https://github.com/devinit/datahub-cms/blob/master/global/bundle.csv#L5);

"Global public goods & NNGOs" was changed to "Global initiatives & NNGOs".

---

#### `entity.csv` which will possibly be merged with `di_id.csv`

We use column `name` on a country's digital profile page.
