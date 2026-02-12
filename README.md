# Analysis of OpenAlex-Walden Snapshot

## Data

OpenAlex: 2025-10-01

Walden: 2026-02-03

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
|  0 | article                 |     15610743 |   15804019 |   193276 | 1.24         |
|  1 | review                  |       693450 |     688378 |    -5072 | -0.73        |
|  2 | paratext                |       427285 |     436417 |     9132 | 2.14         |
|  3 | letter                  |       158396 |     156103 |    -2293 | -1.45        |
|  4 | editorial               |       113624 |     112829 |     -795 | -0.70        |
|  5 | erratum                 |        72718 |      72181 |     -537 | -0.74        |
|  6 | dataset                 |        55909 |       1409 |   -54500 | -97.48       |
|  7 | book-chapter            |        47159 |     103938 |    56779 | 120.40       |
|  8 | reference-entry         |        36094 |      32937 |    -3157 | -8.75        |
|  9 | other                   |        21027 |      20533 |     -494 | -2.35        |
| 10 | book                    |        13115 |      13942 |      827 | 6.31         |
| 11 | retraction              |        13113 |      13189 |       76 | 0.58         |
| 12 | preprint                |        12158 |      23960 |    11802 | 97.07        |
| 13 | report                  |         3512 |       3725 |      213 | 6.06         |
| 14 | supplementary-materials |         2725 |       2724 |       -1 | -0.04        |
| 15 | peer-review             |            4 |          4 |        0 | 0.00         |
| 16 | dissertation            |            1 |        387 |      386 | 38,600.00    |
| 17 | report-component        |            0 |         39 |       39 | inf          |

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
|  0 | journal        |     17281019 |   17486708 |   205689 |         1.19 |
|  1 | None           |      3677588 |    6377383 |  2699795 |        73.41 |
|  2 | repository     |      3593723 |   28534221 | 24940498 |       694    |
|  3 | ebook platform |      2732665 |    1769759 |  -962906 |       -35.24 |
|  4 | book series    |       664146 |     661006 |    -3140 |        -0.47 |
|  5 | conference     |       568721 |     301899 |  -266822 |       -46.92 |
|  6 | other          |           11 |         53 |       42 |       381.82 |
|  7 | igsnCatalog    |            0 |    5370220 |  5370220 |       inf    |
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
|  0 | closed      |      6639794 |    5543491 | -1096303 |       -16.51 |
|  1 | gold        |      4419565 |    2885171 | -1534394 |       -34.72 |
|  2 | hybrid      |      2097302 |    1857336 |  -239966 |       -11.44 |
|  3 | bronze      |      1469074 |    1304379 |  -164695 |       -11.21 |
|  4 | diamond     |      1178478 |    4415079 |  3236601 |       274.64 |
|  5 | green       |       499993 |     487170 |   -12823 |        -2.56 |

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
|  0 | None                                              |      4406230 |    4594482 |   188252 |         4.27 |
|  1 | Elsevier BV                                       |      2488907 |    2486835 |    -2072 |        -0.08 |
|  2 | Multidisciplinary Digital Publishing Institute    |       808028 |     806939 |    -1089 |        -0.13 |
|  3 | Springer Science+Business Media                   |       786127 |     792846 |     6719 |         0.85 |
|  4 | Wiley                                             |       736327 |     733615 |    -2712 |        -0.37 |
|  5 | Taylor & Francis                                  |       367153 |     367567 |      414 |         0.11 |
|  6 | Frontiers Media                                   |       267408 |     267480 |       72 |         0.03 |
|  7 | Oxford University Press                           |       252446 |     252498 |       52 |         0.02 |
|  8 | SAGE Publishing                                   |       228762 |     228486 |     -276 |        -0.12 |
|  9 | Institute of Electrical and Electronics Engineers |       219312 |     218624 |     -688 |        -0.31 |
| 10 | Lippincott Williams & Wilkins                     |       214001 |     215062 |     1061 |         0.5  |
| 11 | American Chemical Society                         |       209069 |     208904 |     -165 |        -0.08 |
| 12 | Nature Portfolio                                  |       168977 |     168786 |     -191 |        -0.11 |
| 13 | BioMed Central                                    |       158854 |     158539 |     -315 |        -0.2  |
| 14 | Springer Nature                                   |       137535 |     138302 |      767 |         0.56 |
| 15 | Royal Society of Chemistry                        |       114134 |     113681 |     -453 |        -0.4  |
| 16 | American Institute of Physics                     |       108662 |     108349 |     -313 |        -0.29 |
| 17 | IOP Publishing                                    |       107898 |     107132 |     -766 |        -0.71 |
| 18 | Cambridge University Press                        |        96301 |      97781 |     1480 |         1.54 |
| 19 | Medknow                                           |        81621 |      81152 |     -469 |        -0.57 |

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
|  0 | Elsevier BV                                       |     99552763 |  109168209 |  9615446 |         9.66 |
|  1 | Multidisciplinary Digital Publishing Institute    |     42306176 |   45172150 |  2865974 |         6.77 |
|  2 | None                                              |     33877276 |   38658424 |  4781148 |        14.11 |
|  3 | Springer Science+Business Media                   |     31926819 |   33223165 |  1296346 |         4.06 |
|  4 | Wiley                                             |     31252965 |   32518444 |  1265479 |         4.05 |
|  5 | Frontiers Media                                   |     15792532 |   16325270 |   532738 |         3.37 |
|  6 | Taylor & Francis                                  |     13892854 |   14536016 |   643162 |         4.63 |
|  7 | American Chemical Society                         |     11340736 |   11397265 |    56529 |         0.5  |
|  8 | Nature Portfolio                                  |      8218701 |    8486299 |   267598 |         3.26 |
|  9 | SAGE Publishing                                   |      7613836 |    7758131 |   144295 |         1.9  |
| 10 | BioMed Central                                    |      7415451 |    7553572 |   138121 |         1.86 |
| 11 | Oxford University Press                           |      6495768 |    7192185 |   696417 |        10.72 |
| 12 | Institute of Electrical and Electronics Engineers |      6484987 |    8255490 |  1770503 |        27.3  |
| 13 | Royal Society of Chemistry                        |      6391983 |    6649286 |   257303 |         4.03 |
| 14 | Springer Nature                                   |      5675102 |    5941032 |   265930 |         4.69 |
| 15 | American Physical Society                         |      3719271 |    3908099 |   188828 |         5.08 |
| 16 | IOP Publishing                                    |      3583768 |    4107631 |   523863 |        14.62 |
| 17 | Lippincott Williams & Wilkins                     |      3025445 |    3487144 |   461699 |        15.26 |
| 18 | Public Library of Science                         |      2888721 |    2976937 |    88216 |         3.05 |
| 19 | American Institute of Physics                     |      2884919 |    3047010 |   162091 |         5.62 |

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
|  0 |               2024 |      5747463 |    5866769 |   119306 |         2.08 |
|  1 |               2023 |      5385802 |    5421376 |    35574 |         0.66 |
|  2 |               2022 |      5170934 |    5204308 |    33374 |         0.65 |
|  4 |               2020 |      4865503 |    4820914 |   -44589 |        -0.92 |
|  3 |               2021 |      5168607 |    5113724 |   -54883 |        -1.06 |

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
|  2 | doaj         |       796361 |    4259521 |  3463160 |       434.87 |
|  0 | crossref     |     16303367 |   16439256 |   135889 |         0.83 |
|  4 | datacite     |        53760 |     138476 |    84716 |       157.58 |
|  3 | arxiv        |       108489 |      66351 |   -42138 |       -38.84 |
|  1 | pubmed       |      4129715 |    4024872 |  -104843 |        -2.54 |

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
|  2 | datacite     |      1580251 |   31919449 | 30339198 | 1,919.90     |
|  3 | doaj         |       840008 |    4639906 |  3799898 | 452.36       |
|  0 | crossref     |     26149607 |   26315869 |   166262 | 0.64         |
|  4 | arxiv        |       659630 |     663984 |     4354 | 0.66         |
|  1 | pubmed       |      4572091 |    4459692 |  -112399 | -2.46        |

### Number of rows per table

|    |   table            |   n_openalex |   n_walden |     change |   pct_change |
|---:|-------------------:|-------------:|-----------:|-----------:|-------------:|
|  0 | authors            |    104942999 |  107541376 |    2598377 |         2.48 |
|  1 | funders            |        32437 |      32437 |          0 |            0 |
|  2 | institutions       |       117731 |     120658 |       2927 |         2.49 |
|  3 | publishers         |        10741 |      10703 |        -38 |        -0.35 |
|  4 | sources            |       260787 |     255250 |      -5537 |        -2.12 |
|  5 | topics             |         4516 |       4516 |          0 |            0 |
|  6 | works              |    271335309 |  479290643 |  207955334 |        76.64 |
|  7 | awards             |            0 |   11746894 |   11746894 |          inf |

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
|  0 | National Natural Science Foundation of China                      |      1463516 |    1457223 |    -6293 |        -0.43 |
|  1 | National Key Research and Development Program of China            |       163991 |     163096 |     -895 |        -0.55 |
|  2 | Japan Society for the Promotion of Science                        |       128342 |     126596 |    -1746 |        -1.36 |
|  3 | National Institutes of Health                                     |       126402 |     125356 |    -1046 |        -0.83 |
|  4 | National Science Foundation                                       |       118946 |     116257 |    -2689 |        -2.26 |
|  5 | Fundamental Research Funds for the Central Universities           |        87326 |      86786 |     -540 |        -0.62 |
|  6 | Deutsche Forschungsgemeinschaft                                   |        86309 |      84276 |    -2033 |        -2.36 |
|  7 | National Research Foundation of Korea                             |        75647 |      75220 |     -427 |        -0.56 |
|  8 | China Postdoctoral Science Foundation                             |        60066 |      59810 |     -256 |        -0.43 |
|  9 | Conselho Nacional de Desenvolvimento Científico e Tecnológico     |        43147 |      42752 |     -395 |        -0.92 |
| 10 | Natural Science Foundation of Shandong Province                   |        39073 |      38954 |     -119 |        -0.3  |
| 11 | Fundação para a Ciência e a Tecnologia                            |        33985 |      33659 |     -326 |        -0.96 |
| 12 | Fundação de Amparo à Pesquisa do Estado de São Paulo              |        33824 |      33570 |     -254 |        -0.75 |
| 13 | Engineering and Physical Sciences Research Council                |        33358 |      87044 |    53686 |       160.94 |
| 14 | Australian Research Council                                       |        28688 |      28317 |     -371 |        -1.29 |
| 15 | Natural Science Foundation of Jiangsu Province                    |        28053 |      27957 |      -96 |        -0.34 |
| 16 | Ministerio de Ciencia e Innovación                                |        27064 |      26772 |     -292 |        -1.08 |
| 17 | Basic and Applied Basic Research Foundation of Guangdong Province |        26863 |      26782 |      -81 |        -0.3  |
| 18 | Natural Sciences and Engineering Research Council of Canada       |        25888 |      25454 |     -434 |        -1.68 |
| 19 | Science and Engineering Research Board                            |        25565 |      25363 |     -202 |        -0.79 |

OAL grant id count: 5,420,652

Walden grant id count: 5,574,901

### Schema changes 

Code for schema evaluation is retrieved from [here](https://github.com/naustica/openalex_schema).

- [Works](Works.ipynb)
- [Funders](Funders.ipynb)
- [Publishers](Publishers.ipynb)
- [Sources](Sources.ipynb)
- [Institutions](Institutions.ipynb)

