When using the data, please cite the following paper:
LaBelle, J., Martinez-Zarzoso, I., Santacreu, A. M. & Yotov, Y. 2023. "Cross-border Patenting, Globalization, and Development," Working Papers 2023-031, Federal Reserve Bank of St. Louis. [https://ideas.repec.org/p/fip/fedlwp/97470.html](https://ideas.repec.org/p/fip/fedlwp/97470.html)

## INPACT-S

#### Variables:
```
appln_auth            - the 2 digit code of the country to which the patent application was filed
appln_auth_iso3       - the 3 digit code of the country to which the patent application was filed
appln_auth_name       - the standardized name of the country to which the patent application was filed
person_ctry_code      - the 2 digit code of the country in which the patent applicant resides
person_ctry_code_iso3 - the 3 digit code of the country in which the patent applicant resides
person_ctry_code_name - the standardized name of the country in which the patent applicant resides
appln_filing_year     - the year in which the specific application being counted was filed
                        (NOTE: it is possible and perhaps common for a technology to be filed in
                        different jurisdictions in different years. In this cases, each are counted
                        in the year that specific application was filed, NOT the year of the first application).
isic_rev3_2           - The 2-digit industry code of the patent according to ISIC Revision 3
patents_applt         - The count of bilateral, industry level, yearly patents counted by applicants country
                        (as opposed to inventors country)
```

#### Data used in the creation of this dataset:

* Patent data -- [PATSTAT Global 2021 - single edition (Autumn)](https://shop.epo.org/en/Data-and-services/PATSTAT/c/patstat?q=%3Arelevance%3Aperiod%3ABI_ANNUAL&text=#)
* IPC - [ISIC crosswalk -- Lybert and Zolas (2014)](https://sites.google.com/site/nikolaszolas/PatentCrosswalk)
* Regional Authority Members -- Original Data
* WIPO Codes -- [WIPO ST.3 Table](https://www.uspto.gov/patents/apply/applying-online/country-codes-wipo-st3-table#heading-2)
* ISO 3-Digit Codes -- [https://www.iban.com/country-codes](https://www.iban.com/country-codes)
* WIPO - ISO3 crosswalk -- Original Data
* WIPO Patent Data -- [WIPO IP Statistics Data Center](https://www3.wipo.int/ipstats/)

#### Summary of the process (full replication scripts available upon request along with all data except that which is propritory such as the raw PATSTAT data):

1) First we take the output of SQL code provided by deRassenfosse & Siegler (2019) and merge it into our data for the purposes of filling in some of the missing country codes from the raw PATSTAT data.

2) Next, we apply our fractional counting method from the after we impute blank person_ctry_code in the raw data using deRassanfosse method

3) Next, we use the crosswalks created by Lybbert and Zolas (2014) to convert IPC codes to ISIC revision 3 2 digit codes

4) Some groups of countries form regional patent authorities which take on the responsibility of reviewing granting patents. However, the end goal of the applicant is to seek enforcement in certain countries that take part in the regional agreement. It is highly unlikely that each county attract patents to the regional authority equally so we attempt to reasonably estimate where each patent is inteded to be enforced. This means assigning an application to a member country based on the country most likely to have attracted that patent.

5) Next, we get rid of bad data. PATSTAT reads in the full-text data from filings. This means typos on the application can make it into the raw data. For example, if someone puts EN instead of UK. It would theoretically be possible to guess the most likely intended authority but we choose to drop them. Here we take WIPO's official country list and merge it in we drop whatever is not found in the official list.

6) Finally, we deal with the blanks. One possibility would be to deal with these similar to how we did regional authorities. The issue with that is which patent data is reported and which is imputed is likely not a random sample. Using a method like that would reinforce bias in the data. We choose to estimate based on WIPO value. In other words, if 25% of patent applications to Canada come from the US in 2015, then we give 25% of our blank Canadian patents to the US. One necessary assumption is that this share holds across industries.

## INPACT-S-Citations

#### Variables:
```
person_ctry_code        - the 2 digit code of the country in which the inventor of the citing patent resides
cited_person_ctry_code  - the 2 digit code of the country in which the inventor of the cited patent resides
isic_rev3_2             - the 2-digit industry code of the citing patent according to ISIC Revision 3
cited_isic_rev3_2       - the 2-digit industry code of the cited patent according to ISIC Revision 3
appln_filing_year       - the year in which the citing patent was filed
cited_appln_filing_year - the year in which the cited patent was filed
citations               - the count of citations. This is at the industry and country level. So every industry
                          and country involved in a citing-cited relationship in an application gets 1 citation.
```

 #### Data used in the creation of this dataset:

* Patent data -- [PATSTAT Global 2021 - single edition (Autumn)](https://shop.epo.org/en/Data-and-services/PATSTAT/c/patstat?q=%3Arelevance%3Aperiod%3ABI_ANNUAL&text=#)
* IPC - [ISIC crosswalk -- Lybert and Zolas (2014)](https://sites.google.com/site/nikolaszolas/PatentCrosswalk)
* Regional Authority Members -- Original Data
* WIPO Codes -- [WIPO ST.3 Table](https://www.uspto.gov/patents/apply/applying-online/country-codes-wipo-st3-table#heading-2)
* [ISO 3-Digit Codes](https://www.iban.com/country-codes)
* WIPO - ISO3 crosswalk -- Original Data
* WIPO Patent Data -- [WIPO IP Statistics Data Center](https://www3.wipo.int/ipstats/)

#### Summary of the process:

This process is very complicated. The scripts are well documented and the best way to learn the process is by reading those carefully.
