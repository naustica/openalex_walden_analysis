# Analysis of OpenAlex-Walden Snapshot

## Data

OpenAlex: 2025-10-01

Walden: 2025-11-05

Publication years: 2022-2024 (unless otherwise stated)

Data was accessed via the Scholarly Data Warehouse of the SUB Göttingen: https://subugoe.github.io/scholcomm_analytics/data.html

## Results

### Document types

```sql
SELECT COUNT(DISTINCT(doi)) AS n, type
FROM {snapshot}
WHERE primary_location.source.type = 'journal' AND publication_year BETWEEN 2022 AND 2024
GROUP BY type
ORDER BY n DESC
```

|    | type                    |   n_openalex |   n_walden |   change | pct_change   |
|---:|:------------------------|-------------:|-----------:|---------:|:-------------|
|  0 | article                 |     15610743 |   16940674 |  1329931 | 8.52         |
|  1 | review                  |       693450 |          6 |  -693444 | -100.00      |
|  2 | paratext                |       427285 |     290217 |  -137068 | -32.08       |
|  3 | letter                  |       158396 |          1 |  -158395 | -100.00      |
|  4 | editorial               |       113624 |          0 |  -113624 | -100.00      |
|  5 | erratum                 |        72718 |          0 |   -72718 | -100.00      |
|  6 | dataset                 |        55909 |       1492 |   -54417 | -97.33       |
|  7 | book-chapter            |        47159 |     105042 |    57883 | 122.74       |
|  8 | reference-entry         |        36094 |      32981 |    -3113 | -8.62        |
|  9 | other                   |        21027 |      21718 |      691 | 3.29         |
| 10 | book                    |        13115 |      14015 |      900 | 6.86         |
| 11 | retraction              |        13113 |          0 |   -13113 | -100.00      |
| 12 | preprint                |        12158 |         63 |   -12095 | -99.48       |
| 13 | report                  |         3512 |       3782 |      270 | 7.69         |
| 14 | supplementary-materials |         2725 |          0 |    -2725 | -100.00      |
| 15 | peer-review             |            4 |          1 |       -3 | -75.00       |
| 16 | dissertation            |            1 |        251 |      250 | 25,000.00    |
| 17 | report-component        |            0 |      12399 |    12399 | inf          |
| 18 | book-section            |            0 |         29 |       29 | inf          |

### Publication types

```sql
SELECT COUNT(DISTINCT(doi)) AS n, primary_location.source.type AS source_type
FROM {snapshot}
WHERE publication_year BETWEEN 2022 AND 2024
GROUP BY source_type
ORDER BY n DESC
```

|    | source_type    |   n_openalex |   n_walden |   change |   pct_change |
|---:|:---------------|-------------:|-----------:|---------:|-------------:|
|  0 | journal        |     17281019 |   17422669 |   141650 |         0.82 |
|  1 | None           |      3677588 |    6648332 |  2970744 |        80.78 |
|  2 | repository     |      3593723 |   35230747 | 31637024 |       880.34 |
|  3 | ebook platform |      2732665 |    1761516 |  -971149 |       -35.54 |
|  4 | book series    |       664146 |     662648 |    -1498 |        -0.23 |
|  5 | conference     |       568721 |     301416 |  -267305 |       -47    |
|  6 | other          |           11 |         63 |       52 |       472.73 |
|  7 | igsnCatalog    |            0 |   11489134 | 11489134 |       inf    |
|  8 | metadata       |            0 |        135 |      135 |       inf    |
|  9 | raidRegistry   |            0 |          4 |        4 |       inf    |

### Open Access Status

```sql
SELECT COUNT(DISTINCT(doi)) AS n, open_access.is_oa, open_access.oa_status
FROM {snapshot}
WHERE primary_location.source.type = 'journal' AND type = 'article' AND publication_year BETWEEN 2022 AND 2024
GROUP BY is_oa, oa_status
ORDER BY n DESC
```

|    | oa_status   |   n_openalex |   n_walden |   change |   pct_change |
|---:|:------------|-------------:|-----------:|---------:|-------------:|
|  0 | closed      |      6396765 |    6419423 |    22658 |         0.35 |
|  1 | gold        |      4155508 |    2982716 | -1172792 |       -28.22 |
|  2 | hybrid      |      2006569 |    1630570 |  -375999 |       -18.74 |
|  3 | bronze      |      1427168 |    1008387 |  -418781 |       -29.34 |
|  4 | diamond     |      1151659 |    4426122 |  3274463 |       284.33 |
|  5 | green       |       473087 |     473677 |      590 |         0.12 |

### Publisher (Top 20)

```sql
SELECT COUNT(DISTINCT(doi)) AS n, primary_location.source.host_organization_name
FROM {snapshot}
WHERE primary_location.source.type = 'journal' AND type = 'article' AND publication_year BETWEEN 2022 AND 2024
GROUP BY host_organization_name
ORDER BY n DESC
```

|    | host_organization_name                            |   n_openalex |   n_walden |   change |   pct_change |
|---:|:--------------------------------------------------|-------------:|-----------:|---------:|-------------:|
|  0 | None                                              |      4344384 |    4566659 |   222275 |         5.12 |
|  1 | Elsevier BV                                       |      2343634 |    2640008 |   296374 |        12.65 |
|  2 | Springer Science+Business Media                   |       735480 |     822804 |    87324 |        11.87 |
|  3 | Multidisciplinary Digital Publishing Institute    |       723526 |     815693 |    92167 |        12.74 |
|  4 | Wiley                                             |       688901 |     812110 |   123209 |        17.88 |
|  5 | Taylor & Francis                                  |       348443 |     373977 |    25534 |         7.33 |
|  6 | Oxford University Press                           |       240608 |     265004 |    24396 |        10.14 |
|  7 | Frontiers Media                                   |       226686 |     288075 |    61389 |        27.08 |
|  8 | Institute of Electrical and Electronics Engineers |       217614 |     223717 |     6103 |         2.8  |
|  9 | SAGE Publishing                                   |       214444 |     236165 |    21721 |        10.13 |
| 10 | American Chemical Society                         |       202341 |     222253 |    19912 |         9.84 |
| 11 | Lippincott Williams & Wilkins                     |       195938 |     229587 |    33649 |        17.17 |
| 12 | Nature Portfolio                                  |       157522 |     179082 |    21560 |        13.69 |
| 13 | BioMed Central                                    |       141830 |     165669 |    23839 |        16.81 |
| 14 | Springer Nature                                   |       126992 |     146910 |    19918 |        15.68 |
| 15 | Royal Society of Chemistry                        |       107209 |     125565 |    18356 |        17.12 |
| 16 | American Institute of Physics                     |       106790 |     109098 |     2308 |         2.16 |
| 17 | IOP Publishing                                    |       106428 |     108494 |     2066 |         1.94 |
| 18 | Cambridge University Press                        |        93186 |     103349 |    10163 |        10.91 |
| 19 | Medknow                                           |        76634 |      84385 |     7751 |        10.11 |

### References per publisher (Top 20)

```sql
SELECT SUM(referenced_works_count) AS n, primary_location.source.host_organization_name
FROM {snapshot}
WHERE primary_location.source.type = 'journal' AND type = 'article' AND publication_year BETWEEN 2022 AND 2024
GROUP BY host_organization_name
ORDER BY n DESC
```

|    | host_organization_name                            |   r_openalex |   r_walden |   change |   pct_change |
|---:|:--------------------------------------------------|-------------:|-----------:|---------:|-------------:|
|  0 | Elsevier BV                                       |     86479521 |  109874976 | 23395455 |        27.05 |
|  1 | Multidisciplinary Digital Publishing Institute    |     32402168 |   45314021 | 12911853 |        39.85 |
|  2 | None                                              |     31983442 |   38616530 |  6633088 |        20.74 |
|  3 | Springer Science+Business Media                   |     27713938 |   33460068 |  5746130 |        20.73 |
|  4 | Wiley                                             |     26884951 |   32793652 |  5908701 |        21.98 |
|  5 | Taylor & Francis                                  |     12210767 |   14588068 |  2377301 |        19.47 |
|  6 | Frontiers Media                                   |     11394387 |   16445860 |  5051473 |        44.33 |
|  7 | American Chemical Society                         |     10245639 |   11423941 |  1178302 |        11.5  |
|  8 | Nature Portfolio                                  |      7382114 |    8530781 |  1148667 |        15.56 |
|  9 | SAGE Publishing                                   |      6766799 |    7798486 |  1031687 |        15.25 |
| 10 | Institute of Electrical and Electronics Engineers |      6315398 |    8274424 |  1959026 |        31.02 |
| 11 | BioMed Central                                    |      5963888 |    7606606 |  1642718 |        27.54 |
| 12 | Oxford University Press                           |      5699999 |    7311860 |  1611861 |        28.28 |
| 13 | Royal Society of Chemistry                        |      5357644 |    6655675 |  1298031 |        24.23 |
| 14 | Springer Nature                                   |      4574818 |    6026293 |  1451475 |        31.73 |
| 15 | American Physical Society                         |      3652182 |    4053047 |   400865 |        10.98 |
| 16 | IOP Publishing                                    |      3399909 |    4160299 |   760390 |        22.37 |
| 17 | American Institute of Physics                     |      2793504 |    3067077 |   273573 |         9.79 |
| 18 | Public Library of Science                         |      2670112 |    2983975 |   313863 |        11.75 |
| 19 | Emerald Publishing Limited                        |      2385213 |    2918749 |   533536 |        22.37 |

### Journal articles per publication year

```sql
SELECT COUNT(DISTINCT(doi)) AS n, publication_year
FROM {snapshot}
WHERE primary_location.source.type = 'journal' AND type = 'article' AND publication_year BETWEEN 2020 AND 2024
GROUP BY publication_year
ORDER BY publication_year DESC
```

|    |   publication_year |   n_openalex |   n_walden |   change |   pct_change |
|---:|-------------------:|-------------:|-----------:|---------:|-------------:|
|  0 |               2024 |      5521460 |    5999348 |   477888 |         8.66 |
|  1 |               2023 |      5154530 |    5591304 |   436774 |         8.47 |
|  2 |               2022 |      4934761 |    5350076 |   415315 |         8.42 |
|  3 |               2021 |      4935345 |    5307559 |   372214 |         7.54 |
|  4 |               2020 |      4662309 |    5013208 |   350899 |         7.53 |

### Number of rows per table

|    |   tables           |   n_openalex |   n_walden |     change |   pct_change |
|---:|-------------------:|-------------:|-----------:|-----------:|-------------:|
|  0 | authors            |    104942999 |  115794829 |   10851830 |        10.34 |
|  1 | funders            |        32437 |      32437 |          0 |            0 |
|  2 | institutions       |       117731 |     102539 |     -15192 |       -12.90 |
|  3 | publishers         |        10741 |         16 |     -10725 |       -99,85 |
|  4 | sources            |       260787 |     255250 |      -5537 |        -2.12 |
|  5 | topics             |         4516 |       4516 |          0 |            0 |
|  6 | works              |    271335309 |  463042077 |  191706768 |        70.65 |

