# Analysis of OpenAlex-Walden Snapshot

## Data

OpenAlex: 2025-10-01

Walden: 2026-01-14

Publication years: 2022-2024 (unless otherwise stated)

Data was accessed via the Scholarly Data Warehouse of the SUB Göttingen: https://subugoe.github.io/scholcomm_analytics/data.html

Analysis code can be found [here](Codebook.ipynb).

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
|  0 | article                 |     15610743 |   15789640 |   178897 | 1.15         |
|  1 | review                  |       693450 |     688214 |    -5236 | -0.76        |
|  2 | paratext                |       427285 |     435787 |     8502 | 1.99         |
|  3 | letter                  |       158396 |     156083 |    -2313 | -1.46        |
|  4 | editorial               |       113624 |     112817 |     -807 | -0.71        |
|  5 | erratum                 |        72718 |      72179 |     -539 | -0.74        |
|  6 | dataset                 |        55909 |       1410 |   -54499 | -97.48       |
|  7 | book-chapter            |        47159 |     103777 |    56618 | 120.06       |
|  8 | reference-entry         |        36094 |      32940 |    -3154 | -8.74        |
|  9 | other                   |        21027 |      20564 |     -463 | -2.20        |
| 10 | book                    |        13115 |      13952 |      837 | 6.38         |
| 11 | retraction              |        13113 |      13189 |       76 | 0.58         |
| 12 | preprint                |        12158 |      24106 |    11948 | 98.27        |
| 13 | report                  |         3512 |       3709 |      197 | 5.61         |
| 14 | supplementary-materials |         2725 |       2724 |       -1 | -0.04        |
| 15 | peer-review             |            4 |          4 |        0 | 0.00         |
| 16 | dissertation            |            1 |        365 |      364 | 36,400.00    |
| 17 | report-component        |            0 |         26 |       26 | inf          |

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
|  0 | journal        |     17281019 |   17471480 |   190461 |         1.1  |
|  1 | None           |      3677588 |    6219639 |  2542051 |        69.12 |
|  2 | repository     |      3593723 |   28683477 | 25089754 |       698.15 |
|  3 | ebook platform |      2732665 |    1768359 |  -964306 |       -35.29 |
|  4 | book series    |       664146 |     661015 |    -3131 |        -0.47 |
|  5 | conference     |       568721 |     301813 |  -266908 |       -46.93 |
|  6 | other          |           11 |         54 |       43 |       390.91 |
|  7 | igsnCatalog    |            0 |    5336999 |  5336999 |       inf    |
|  8 | metadata       |            0 |        135 |      135 |       inf    |
|  9 | raidRegistry   |            0 |          4 |        4 |       inf    |

### Open Access Status

```sql
SELECT COUNT(DISTINCT(doi)) AS n, open_access.is_oa, open_access.oa_status
FROM {snapshot}
WHERE primary_location.source.type = 'journal' AND publication_year BETWEEN 2022 AND 2024 AND (type = 'article' OR type = 'review')
GROUP BY is_oa, oa_status
ORDER BY n DESC
```

|    | oa_status   |   n_openalex |   n_walden |   change |   pct_change |
|---:|:------------|-------------:|-----------:|---------:|-------------:|
|  0 | closed      |      6639794 |    5646375 |  -993419 |       -14.96 |
|  1 | gold        |      4419565 |    2884933 | -1534632 |       -34.72 |
|  2 | hybrid      |      2097302 |    1848233 |  -249069 |       -11.88 |
|  3 | bronze      |      1469074 |    1197094 |  -271980 |       -18.51 |
|  4 | diamond     |      1178478 |    4412769 |  3234291 |       274.45 |
|  5 | green       |       499993 |     488679 |   -11314 |        -2.26 |

### Publisher (Top 20)

```sql
SELECT COUNT(DISTINCT(doi)) AS n, primary_location.source.host_organization_name
FROM {snapshot}
WHERE primary_location.source.type = 'journal' AND publication_year BETWEEN 2022 AND 2024 AND (type = 'article' OR type = 'review')
GROUP BY host_organization_name
ORDER BY n DESC
```

|    | host_organization_name                            |   n_openalex |   n_walden |   change |   pct_change |
|---:|:--------------------------------------------------|-------------:|-----------:|---------:|-------------:|
|  0 | None                                              |      4406230 |    4583234 |   177004 |         4.02 |
|  1 | Elsevier BV                                       |      2488907 |    2486832 |    -2075 |        -0.08 |
|  2 | Multidisciplinary Digital Publishing Institute    |       808028 |     806939 |    -1089 |        -0.13 |
|  3 | Springer Science+Business Media                   |       786127 |     792822 |     6695 |         0.85 |
|  4 | Wiley                                             |       736327 |     733618 |    -2709 |        -0.37 |
|  5 | Taylor & Francis                                  |       367153 |     367539 |      386 |         0.11 |
|  6 | Frontiers Media                                   |       267408 |     267480 |       72 |         0.03 |
|  7 | Oxford University Press                           |       252446 |     252470 |       24 |         0.01 |
|  8 | SAGE Publishing                                   |       228762 |     228465 |     -297 |        -0.13 |
|  9 | Institute of Electrical and Electronics Engineers |       219312 |     218626 |     -686 |        -0.31 |
| 10 | Lippincott Williams & Wilkins                     |       214001 |     215062 |     1061 |         0.5  |
| 11 | American Chemical Society                         |       209069 |     208904 |     -165 |        -0.08 |
| 12 | Nature Portfolio                                  |       168977 |     168784 |     -193 |        -0.11 |
| 13 | BioMed Central                                    |       158854 |     158540 |     -314 |        -0.2  |
| 14 | Springer Nature                                   |       137535 |     138307 |      772 |         0.56 |
| 15 | Royal Society of Chemistry                        |       114134 |     113681 |     -453 |        -0.4  |
| 16 | American Institute of Physics                     |       108662 |     108348 |     -314 |        -0.29 |
| 17 | IOP Publishing                                    |       107898 |     107132 |     -766 |        -0.71 |
| 18 | Cambridge University Press                        |        96301 |      97682 |     1381 |         1.43 |
| 19 | Medknow                                           |        81621 |      81118 |     -503 |        -0.62 |

### References per publisher (Top 20)

```sql
SELECT SUM(referenced_works_count) AS n, primary_location.source.host_organization_name
FROM {snapshot}
WHERE primary_location.source.type = 'journal' AND publication_year BETWEEN 2022 AND 2024 AND (type = 'article' OR type = 'review')
GROUP BY host_organization_name
ORDER BY n DESC
```

|    | host_organization_name                            |   n_openalex |   n_walden |   change |   pct_change |
|---:|:--------------------------------------------------|-------------:|-----------:|---------:|-------------:|
|  0 | Elsevier BV                                       |     99552763 |  109168132 |  9615369 |         9.66 |
|  1 | Multidisciplinary Digital Publishing Institute    |     42306176 |   45172150 |  2865974 |         6.77 |
|  2 | None                                              |     33877276 |   38620785 |  4743509 |        14    |
|  3 | Springer Science+Business Media                   |     31926819 |   33222209 |  1295390 |         4.06 |
|  4 | Wiley                                             |     31252965 |   32518662 |  1265697 |         4.05 |
|  5 | Frontiers Media                                   |     15792532 |   16325262 |   532730 |         3.37 |
|  6 | Taylor & Francis                                  |     13892854 |   14534769 |   641915 |         4.62 |
|  7 | American Chemical Society                         |     11340736 |   11397264 |    56528 |         0.5  |
|  8 | Nature Portfolio                                  |      8218701 |    8486179 |   267478 |         3.25 |
|  9 | SAGE Publishing                                   |      7613836 |    7757894 |   144058 |         1.89 |
| 10 | BioMed Central                                    |      7415451 |    7553623 |   138172 |         1.86 |
| 11 | Oxford University Press                           |      6495768 |    7191019 |   695251 |        10.7  |
| 12 | Institute of Electrical and Electronics Engineers |      6484987 |    8254147 |  1769160 |        27.28 |
| 13 | Royal Society of Chemistry                        |      6391983 |    6649183 |   257200 |         4.02 |
| 14 | Springer Nature                                   |      5675102 |    5941079 |   265977 |         4.69 |
| 15 | American Physical Society                         |      3719271 |    3908141 |   188870 |         5.08 |
| 16 | IOP Publishing                                    |      3583768 |    4107191 |   523423 |        14.61 |
| 17 | Lippincott Williams & Wilkins                     |      3025445 |    3487143 |   461698 |        15.26 |
| 18 | Public Library of Science                         |      2888721 |    2977061 |    88340 |         3.06 |
| 19 | American Institute of Physics                     |      2884919 |    3047002 |   162083 |         5.62 |

### Journal articles per publication year

```sql
SELECT COUNT(DISTINCT(doi)) AS n, publication_year
FROM {snapshot}
WHERE primary_location.source.type = 'journal' AND publication_year BETWEEN 2022 AND 2024 AND (type = 'article' OR type = 'review')
GROUP BY publication_year
ORDER BY publication_year DESC
```

|    |   publication_year |   n_openalex |   n_walden |   change |   pct_change |
|---:|-------------------:|-------------:|-----------:|---------:|-------------:|
|  0 |               2024 |      5747463 |    5858802 |   111339 |         1.94 |
|  1 |               2023 |      5385802 |    5417422 |    31620 |         0.59 |
|  2 |               2022 |      5170934 |    5201685 |    30751 |         0.59 |
|  4 |               2020 |      4865503 |    4818824 |   -46679 |        -0.96 |
|  3 |               2021 |      5168607 |    5111484 |   -57123 |        -1.11 |

### Indexed in (only primary_location.source.type = 'journal')

```sql
SELECT COUNT(DISTINCT(doi)) AS n, indexed_in
FROM {snapshot}
WHERE primary_location.source.type = 'journal' AND publication_year BETWEEN 2022 AND 2024 AND (type = 'article' OR type = 'review')
GROUP BY indexed_in
ORDER BY n DESC
```

|    | indexed_in   |   n_openalex |   n_walden |   change |   pct_change |
|---:|:-------------|-------------:|-----------:|---------:|-------------:|
|  2 | doaj         |       796361 |    4250796 |  3454435 |       433.78 |
|  0 | crossref     |     16303367 |   16424638 |   121271 |         0.74 |
|  4 | datacite     |        53760 |     138507 |    84747 |       157.64 |
|  3 | arxiv        |       108489 |      66263 |   -42226 |       -38.92 |
|  1 | pubmed       |      4129715 |    4016339 |  -113376 |        -2.75 |

### Indexed in (all)

```sql
SELECT COUNT(DISTINCT(doi)) AS n, indexed_in
FROM {snapshot}
WHERE publication_year BETWEEN 2022 AND 2024
GROUP BY indexed_in
ORDER BY n DESC
```

|    | indexed_in   |   n_openalex |   n_walden |   change | pct_change   |
|---:|:-------------|-------------:|-----------:|---------:|:-------------|
|  2 | datacite     |      1580251 |   31878995 | 30298744 | 1,917.34     |
|  3 | doaj         |       840008 |    4629846 |  3789838 | 451.17       |
|  0 | crossref     |     26149607 |   26296666 |   147059 | 0.56         |
|  4 | arxiv        |       659630 |     664824 |     5194 | 0.79         |
|  1 | pubmed       |      4572091 |    4451665 |  -120426 | -2.63        |

### Number of rows per table

|    |   table            |   n_openalex |   n_walden |     change |   pct_change |
|---:|-------------------:|-------------:|-----------:|-----------:|-------------:|
|  0 | authors            |    104942999 |  106100192 |    1157193 |         1.10 |
|  1 | funders            |        32437 |      32437 |          0 |            0 |
|  2 | institutions       |       117731 |     102709 |     -15022 |       -12.76 |
|  3 | publishers         |        10741 |         16 |     -10725 |       -99.85 |
|  4 | sources            |       260787 |     255250 |      -5537 |        -2.12 |
|  5 | topics             |         4516 |       4516 |          0 |            0 |
|  6 | works              |    271335309 |  476951866 |  205616557 |        75.78 |

### Grants (Top 20)

```sql
SELECT COUNT(grant.id|grant.award_id) AS n, grant.funder_display_name
FROM {snapshot}, UNNEST(awards|grants) AS grant
WHERE primary_location.source.type = 'journal' AND publication_year BETWEEN 2022 AND 2024 AND (type = 'article' OR type = 'review')
GROUP BY funder_display_name
ORDER BY n DESC
```

|    | funder_display_name                                               |   n_openalex |   n_walden |   change |   pct_change |
|---:|:------------------------------------------------------------------|-------------:|-----------:|---------:|-------------:|
|  0 | National Natural Science Foundation of China                      |      1463516 |    1456078 |    -7438 |        -0.51 |
|  1 | National Key Research and Development Program of China            |       163991 |     162800 |    -1191 |        -0.73 |
|  2 | Japan Society for the Promotion of Science                        |       128342 |     126240 |    -2102 |        -1.64 |
|  3 | National Institutes of Health                                     |       126402 |     121667 |    -4735 |        -3.75 |
|  4 | National Science Foundation                                       |       118946 |     115302 |    -3644 |        -3.06 |
|  5 | Fundamental Research Funds for the Central Universities           |        87326 |      86439 |     -887 |        -1.02 |
|  6 | Deutsche Forschungsgemeinschaft                                   |        86309 |      83882 |    -2427 |        -2.81 |
|  7 | National Research Foundation of Korea                             |        75647 |      75023 |     -624 |        -0.82 |
|  8 | China Postdoctoral Science Foundation                             |        60066 |      59785 |     -281 |        -0.47 |
|  9 | Conselho Nacional de Desenvolvimento Científico e Tecnológico     |        43147 |      42657 |     -490 |        -1.14 |
| 10 | Natural Science Foundation of Shandong Province                   |        39073 |      38915 |     -158 |        -0.4  |
| 11 | Fundação para a Ciência e a Tecnologia                            |        33985 |      33650 |     -335 |        -0.99 |
| 12 | Fundação de Amparo à Pesquisa do Estado de São Paulo              |        33824 |      33562 |     -262 |        -0.77 |
| 13 | Engineering and Physical Sciences Research Council                |        33358 |      66215 |    32857 |        98.5  |
| 14 | Australian Research Council                                       |        28688 |      28250 |     -438 |        -1.53 |
| 15 | Natural Science Foundation of Jiangsu Province                    |        28053 |      27929 |     -124 |        -0.44 |
| 16 | Ministerio de Ciencia e Innovación                                |        27064 |      26744 |     -320 |        -1.18 |
| 17 | Basic and Applied Basic Research Foundation of Guangdong Province |        26863 |      26764 |      -99 |        -0.37 |
| 18 | Natural Sciences and Engineering Research Council of Canada       |        25888 |      24909 |     -979 |        -3.78 |
| 19 | Science and Engineering Research Board                            |        25565 |      25335 |     -230 |        -0.9  |

OAL grant id count: 5,420,652

Walden grant id count: 5,506,984

### Schema changes 

Code for schema evaluation is retrieved from [here](https://github.com/naustica/openalex_schema).

- [Works](Works.ipynb)
- [Funders](Funders.ipynb)
- [Publishers](Publishers.ipynb)
- [Sources](Sources.ipynb)
- [Institutions](Institutions.ipynb)

