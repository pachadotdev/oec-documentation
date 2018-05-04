# Data Availability

The datasets I have prepared were obtained from UN COMTRADE while Alexander Simoes was in charge of wrangling CID data. I did cover different periods of years according to the data availability.

## Yearly data

|Classification |Availability|
|---------------|------------|
|HS rev 1992    |1992 – 2016 |
|HS rev 1996    |1996 – 2016 |
|HS rev 2002    |2002 – 2016 |
|HS rev 2007    |2007 – 2016 |
|SITC rev 2     |1990 – 2016 |

<!--
## Monthly data

UN Comtrade provides monthly data for the period Jan, 2010 - Dec, 2016. That data is provides as is, and reporting agencies use different HS classifications from HS 92 to HS17.

Raw monthly data mixes different HS codes under one column. Provided that goes against Tidy Data I used official correspondences to generate a column containing HS 07 codes only, and from there I generated tidy datasets for the same HS revisions as yearly data.

SITC rev 2 deserves special attention. SITC rev 2 classification maximum detail considers five digits codes. The correspondence between SITC and HS codes defines both four and six digits HS codes as SITC equivalents. The problem about this conversion is that four digits SITC codes englobe six digits SITC codes (i.e "5711" is an aggrupation of "57111" and "57112" that are plastic products).

To obtain consistent tidy datasets with SITC codes I aggregated the trade and limited the depth level to four digits.
-->
