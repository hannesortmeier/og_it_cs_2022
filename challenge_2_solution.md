# IT-CS 2022 - Bei 100 Mrd. Clicksteam-Events & Backend-Daten den Überblick wahren? Mit SQL geht’s - Eine Coding-Challenge
## Challenge 2

Täglich liefern alle OTTO Group Gesellschaften ihren aktuellen Stand an Produktstammdaten an unseren Datenpool.
In der Tabelle `oghub-analytics.it_cs_2022.skufeed` befinden sich alle Produktstammdaten von Otto aus dem Jahr 2021.
Die Tabelle beinhaltet 3,974,820,167 Einträge.

Berechne den aktuellsten Stand für jede SKU (**S**tock**K**eeping**U**nit, eindeutig identifiziert über die skuID) aus den
Produktstammdaten (`oghub-analytics.it_cs_2022.skufeed`) für den Monat Dezember. Ignoriere die 111,610 Produkte aus der Tabelle
`oghub-analytics.it_cs_2022.ignored_products`.


So könnte der Output einer Lösung aussehen:

| productID              | skuID                           | productName                                                              | loadDate   |
| ---------------------- | ------------------------------- | ------------------------------------------------------------------------ | ---------- |
| EC0101-1098453998      | EC0101-1098488224               | SO NOS Jeans Keith                                                       | 2021-12-31 |
| EC0101-1546575890      | EC0101-1546575895               | Tapered-fit-Jeans,LUKA                                                   | 2021-12-31 |
| EC0101-S0G2O0CB        | EC0101-S0G2O0CBBOYC             | Hattric 5-Pocket-Jeans »HATTRIC HARRIS black rinsed 688745 6348.07«      | 2021-12-31 |
| EC0101-S0I0A0OV        | EC0101-S0I0A0OV8U4A             | FORCE Radtrikot »ROSE, Damen Jersey«                                     | 2021-12-31 |
| EC0101-S0K1V0W5        | EC0101-S0K1V0W5KI6Q             | Brandit Cargohose                                                        | 2021-12-20 |
| EC0101-S0N290Q1        | EC0101-S0N290Q1O1A5             | Abakuhaus Duschvorhang »Moderner Digitaldruck mit 12 Haken auf ...       | 2021-12-31 |
| EC0101-S0Z2605Z        | EC0101-S0Z2605ZL4TV             | CALVENDO Puzzle »CALVENDO Puzzle Homeoffice: Endlich am Arbeitsplat« ... | 2021-12-31 |
| EC0601-AKLBB1513460597 | EC0601-3987682551-0-1513460597  | Primaflor-Ideen in Textil Läufer »GIN«, rechteckig, 6 mm Höhe, ...       | 2021-12-06 |
| EC0601-AKLBB1303858774 | EC0601-1032274899-34-1303858774 | Alba Moda Jerseyblazer, mit aufwändiger Verarbeitung                     | 2021-12-31 |


## Lösungsbeispiel
```SQL
SELECT 
  productID
  , skuID
  , productName
  , loadDate
FROM `oghub-analytics.it_cs_2022.skufeed`
WHERE productId NOT IN (SELECT productID FROM `oghub-analytics.it_cs_2022.ignored_products`)
AND loadDate >= "2021-12-01"
QUALIFY ROW_NUMBER() OVER(PARTITION BY skuID ORDER BY loadDate DESC) = 1;

-- equal solution

WITH newestSKU AS (
  SELECT 
    skuID
    , MAX(loadDate) as loadDate
  FROM oghub-analytics.it_cs_2022.skufeed
  WHERE loadDate >= "2021-12-01"
  GROUP BY skuID
)

SELECT COUNT(DISTINCT sku.skuID)
FROM `oghub-analytics.it_cs_2022.skufeed` AS sku
JOIN newestSKU AS nsku
  ON nsku.skuId = sku.skuID
  AND nsku.loadDate = sku.loadDate
WHERE sku.productId NOT IN (SELECT productID FROM `oghub-analytics.it_cs_2022.ignored_products`)
AND sku.loadDate >= "2021-12-01";
```