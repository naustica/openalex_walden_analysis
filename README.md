# Analysis of OpenAlex-Walden Snapshot

## Data

OpenAlex: 2025-10-01

Walden: 2026-02-25

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
|  0 | article                 |     15610743 |   15976160 |   365417 | 2.34         |
|  1 | review                  |       693450 |     688638 |    -4812 | -0.69        |
|  2 | paratext                |       427285 |     437600 |    10315 | 2.41         |
|  3 | letter                  |       158396 |     156121 |    -2275 | -1.44        |
|  4 | editorial               |       113624 |     112899 |     -725 | -0.64        |
|  5 | erratum                 |        72718 |      72193 |     -525 | -0.72        |
|  6 | dataset                 |        55909 |       2702 |   -53207 | -95.17       |
|  7 | book-chapter            |        47159 |     104039 |    56880 | 120.61       |
|  8 | reference-entry         |        36094 |      32937 |    -3157 | -8.75        |
|  9 | other                   |        21027 |      21654 |      627 | 2.98         |
| 10 | book                    |        13115 |      14002 |      887 | 6.76         |
| 11 | retraction              |        13113 |      13189 |       76 | 0.58         |
| 12 | preprint                |        12158 |      24151 |    11993 | 98.64        |
| 13 | report                  |         3512 |       3930 |      418 | 11.90        |
| 14 | supplementary-materials |         2725 |       2724 |       -1 | -0.04        |
| 15 | peer-review             |            4 |         29 |       25 | 625.00       |
| 16 | dissertation            |            1 |        619 |      618 | 61,800.00    |
| 17 | report-component        |            0 |         39 |       39 | inf          |
| 18 | standard                |            0 |          4 |        4 | inf          |
| 19 | grant                   |            0 |          2 |        2 | inf          |
| 20 | software                |            0 |          0 |        0 | nan          |

### Publication types

```sql
SELECT COUNT(DISTINCT(doi)) AS n, primary_location.source.type AS source_type
FROM {snapshot}
WHERE publication_year BETWEEN 2022 AND 2024
GROUP BY source_type
ORDER BY n DESC
```

|    | source_type    |   n_openalex |   n_walden |   change | pct_change   |
|---:|:---------------|-------------:|-----------:|---------:|:-------------|
|  0 | journal        |     17281019 |   17663488 |   382469 | 2.21         |
|  1 | None           |      3677588 |    5478614 |  1801026 | 48.97        |
|  2 | repository     |      3593723 |   29302806 | 25709083 | 715.39       |
|  3 | ebook platform |      2732665 |    1771976 |  -960689 | -35.16       |
|  4 | book series    |       664146 |     661063 |    -3083 | -0.46        |
|  5 | conference     |       568721 |     301907 |  -266814 | -46.91       |
|  6 | other          |           11 |        169 |      158 | 1,436.36     |
|  7 | igsnCatalog    |            0 |    5426895 |  5426895 | inf          |
|  8 | metadata       |            0 |        135 |      135 | inf          |
|  9 | raidRegistry   |            0 |          4 |        4 | inf          |

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
|  0 | closed      |      6639794 |    5514635 | -1125159 |       -16.95 |
|  1 | gold        |      4419565 |    2867621 | -1551944 |       -35.12 |
|  2 | hybrid      |      2097302 |    1859507 |  -237795 |       -11.34 |
|  3 | bronze      |      1469074 |    1333019 |  -136055 |        -9.26 |
|  4 | diamond     |      1178478 |    4418083 |  3239605 |       274.9  |
|  5 | green       |       499993 |     672226 |   172233 |        34.45 |

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
|  0 | None                                              |      4406230 |    4763280 |   357050 |         8.1  |
|  1 | Elsevier BV                                       |      2488907 |    2486847 |    -2060 |        -0.08 |
|  2 | Multidisciplinary Digital Publishing Institute    |       808028 |     806829 |    -1199 |        -0.15 |
|  3 | Springer Science+Business Media                   |       786127 |     792855 |     6728 |         0.86 |
|  4 | Wiley                                             |       736327 |     734183 |    -2144 |        -0.29 |
|  5 | Taylor & Francis                                  |       367153 |     367611 |      458 |         0.12 |
|  6 | Frontiers Media                                   |       267408 |     267388 |      -20 |        -0.01 |
|  7 | Oxford University Press                           |       252446 |     252493 |       47 |         0.02 |
|  8 | SAGE Publishing                                   |       228762 |     228461 |     -301 |        -0.13 |
|  9 | Institute of Electrical and Electronics Engineers |       219312 |     218640 |     -672 |        -0.31 |
| 10 | Lippincott Williams & Wilkins                     |       214001 |     215063 |     1062 |         0.5  |
| 11 | American Chemical Society                         |       209069 |     208904 |     -165 |        -0.08 |
| 12 | Nature Portfolio                                  |       168977 |     168784 |     -193 |        -0.11 |
| 13 | BioMed Central                                    |       158854 |     158537 |     -317 |        -0.2  |
| 14 | Springer Nature                                   |       137535 |     138296 |      761 |         0.55 |
| 15 | Royal Society of Chemistry                        |       114134 |     113680 |     -454 |        -0.4  |
| 16 | American Institute of Physics                     |       108662 |     108349 |     -313 |        -0.29 |
| 17 | IOP Publishing                                    |       107898 |     107131 |     -767 |        -0.71 |
| 18 | Cambridge University Press                        |        96301 |      97876 |     1575 |         1.64 |
| 19 | Medknow                                           |        81621 |      81182 |     -439 |        -0.54 |

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
|  0 | Elsevier BV                                       |     99552763 |  109168459 |  9615696 |         9.66 |
|  1 | Multidisciplinary Digital Publishing Institute    |     42306176 |   45172151 |  2865975 |         6.77 |
|  2 | None                                              |     33877276 |   38782797 |  4905521 |        14.48 |
|  3 | Springer Science+Business Media                   |     31926819 |   33224278 |  1297459 |         4.06 |
|  4 | Wiley                                             |     31252965 |   32518751 |  1265786 |         4.05 |
|  5 | Frontiers Media                                   |     15792532 |   16325279 |   532747 |         3.37 |
|  6 | Taylor & Francis                                  |     13892854 |   14536495 |   643641 |         4.63 |
|  7 | American Chemical Society                         |     11340736 |   11397270 |    56534 |         0.5  |
|  8 | Nature Portfolio                                  |      8218701 |    8486301 |   267600 |         3.26 |
|  9 | SAGE Publishing                                   |      7613836 |    7758086 |   144250 |         1.89 |
| 10 | BioMed Central                                    |      7415451 |    7553576 |   138125 |         1.86 |
| 11 | Oxford University Press                           |      6495768 |    7192785 |   697017 |        10.73 |
| 12 | Institute of Electrical and Electronics Engineers |      6484987 |    8259944 |  1774957 |        27.37 |
| 13 | Royal Society of Chemistry                        |      6391983 |    6649246 |   257263 |         4.02 |
| 14 | Springer Nature                                   |      5675102 |    5941037 |   265935 |         4.69 |
| 15 | American Physical Society                         |      3719271 |    3908144 |   188873 |         5.08 |
| 16 | IOP Publishing                                    |      3583768 |    4107871 |   524103 |        14.62 |
| 17 | Lippincott Williams & Wilkins                     |      3025445 |    3487544 |   462099 |        15.27 |
| 18 | Public Library of Science                         |      2888721 |    2976937 |    88216 |         3.05 |
| 19 | American Institute of Physics                     |      2884919 |    3047011 |   162092 |         5.62 |

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
|  0 |               2024 |      5747463 |    5931313 |   183850 |         3.2  |
|  1 |               2023 |      5385802 |    5476020 |    90218 |         1.68 |
|  2 |               2022 |      5170934 |    5257499 |    86565 |         1.67 |
|  4 |               2020 |      4865503 |    4867745 |     2242 |         0.05 |
|  3 |               2021 |      5168607 |    5162223 |    -6384 |        -0.12 |

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
|  2 | doaj         |       796361 |    4421730 |  3625369 |       455.24 |
|  0 | crossref     |     16303367 |   16453191 |   149824 |         0.92 |
|  4 | datacite     |        53760 |     168713 |   114953 |       213.83 |
|  3 | arxiv        |       108489 |      66427 |   -42062 |       -38.77 |
|  1 | pubmed       |      4129715 |    4026276 |  -103439 |        -2.5  |

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
|  2 | datacite     |      1580251 |   32002824 | 30422573 | 1,925.17     |
|  3 | doaj         |       840008 |    4647582 |  3807574 | 453.28       |
|  0 | crossref     |     26149607 |   26342128 |   192521 | 0.74         |
|  4 | arxiv        |       659630 |     663611 |     3981 | 0.60         |
|  1 | pubmed       |      4572091 |    4461061 |  -111030 | -2.43        |

### Number of rows per table

|    |   table            |   n_openalex |   n_walden |     change |   pct_change |
|---:|-------------------:|-------------:|-----------:|-----------:|-------------:|
|  0 | authors            |    104942999 |  109695206 |    4752207 |         4.53 |
|  1 | funders            |        32437 |      32437 |          0 |            0 |
|  2 | institutions       |       117731 |     120658 |       2927 |         2.49 |
|  3 | publishers         |        10741 |      10703 |        -38 |        -0.35 |
|  4 | sources            |       260787 |     279377 |      18590 |         7.13 |
|  5 | topics             |         4516 |       4516 |          0 |            0 |
|  6 | works              |    271335309 |  482451464 |  211116155 |        77.81 |
|  7 | awards             |            0 |   11823532 |   11823532 |          inf |

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
|  0 | National Natural Science Foundation of China                      |      1463516 |    1457241 |    -6275 |        -0.43 |
|  1 | National Key Research and Development Program of China            |       163991 |     163097 |     -894 |        -0.55 |
|  2 | Japan Society for the Promotion of Science                        |       128342 |     126599 |    -1743 |        -1.36 |
|  3 | National Institutes of Health                                     |       126402 |     125357 |    -1045 |        -0.83 |
|  4 | National Science Foundation                                       |       118946 |     116270 |    -2676 |        -2.25 |
|  5 | Fundamental Research Funds for the Central Universities           |        87326 |      86788 |     -538 |        -0.62 |
|  6 | Deutsche Forschungsgemeinschaft                                   |        86309 |      84312 |    -1997 |        -2.31 |
|  7 | National Research Foundation of Korea                             |        75647 |      75223 |     -424 |        -0.56 |
|  8 | China Postdoctoral Science Foundation                             |        60066 |      59811 |     -255 |        -0.42 |
|  9 | Conselho Nacional de Desenvolvimento Científico e Tecnológico     |        43147 |      42755 |     -392 |        -0.91 |
| 10 | Natural Science Foundation of Shandong Province                   |        39073 |      38954 |     -119 |        -0.3  |
| 11 | Fundação para a Ciência e a Tecnologia                            |        33985 |      33664 |     -321 |        -0.94 |
| 12 | Fundação de Amparo à Pesquisa do Estado de São Paulo              |        33824 |      33573 |     -251 |        -0.74 |
| 13 | Engineering and Physical Sciences Research Council                |        33358 |      87046 |    53688 |       160.94 |
| 14 | Australian Research Council                                       |        28688 |      28320 |     -368 |        -1.28 |
| 15 | Natural Science Foundation of Jiangsu Province                    |        28053 |      27957 |      -96 |        -0.34 |
| 16 | Ministerio de Ciencia e Innovación                                |        27064 |      26782 |     -282 |        -1.04 |
| 17 | Basic and Applied Basic Research Foundation of Guangdong Province |        26863 |      26782 |      -81 |        -0.3  |
| 18 | Natural Sciences and Engineering Research Council of Canada       |        25888 |      25456 |     -432 |        -1.67 |
| 19 | Science and Engineering Research Board                            |        25565 |      25365 |     -200 |        -0.78 |

OAL grant id count: 5,420,652

Walden grant id count: 5,575,257

### Schema changes 

Code for schema evaluation is retrieved from [here](https://github.com/naustica/openalex_schema).

- [Works](Works.ipynb)
- [Funders](Funders.ipynb)
- [Publishers](Publishers.ipynb)
- [Sources](Sources.ipynb)
- [Institutions](Institutions.ipynb)

