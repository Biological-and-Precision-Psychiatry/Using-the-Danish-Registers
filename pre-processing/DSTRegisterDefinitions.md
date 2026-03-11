Definition of Study Population and Variables
=====================

> - **Status:** DRAFT document
> - **Author:** Clara S. Grønkjær, Rune H. B. Christensen, Eva N. S. Wandall
> - **Date:** 2024-12-18
> - **Last edit**: 2026-03-11 (ENSW)

---


# Purpose of This Document

This document is intended for researchers initiating a register-based study using data from Statistics Denmark (DK: Danmarks Statistik, DST) and Sundhedsdatastyrelsen.

It describes:

* The definition of the study population
* Implemented variables derived from national registers
* Known methodological issues
* Points requiring analytical decisions

Potential methodological concerns are marked as:

⚠ **Possible Problems**

Proposed mitigations are marked as:

✔ **Possible Solutions**

Additional resources are marked as:

🕮 **Resources**


---

# Table of Contents

1. [Study Population and Flags](#study-population-and-flags)
2. [Income](#income)
3. [Education](#education)
4. [Employment / Work Status](#employment--work-status)
5. [Hospital diagnoses](#hospital-diagnoses)
6. [Psychiatric Diagnoses](#psychiatric-diagnoses)
7. [Parental Psychiatric Disorder](#parental-psychiatric-disorder)
8. [Charlson Comorbidity Index (CCI)](#charlson-comorbidity-index-(cci))
9. [Prescription Registry](#prescription-registry)
10. [Laboratory Results](#laboratory-results)

---

# Study Population and Flags

## Valid Observation Flag: `i_valid`

Defined in: `10_make_population.rmd`

The flag `i_valid = 1` when all of the following conditions are met:

```r
i_cprstart == 1 &
f_dkstatus != "exit" &
is_dead == 0 &
f_cprsens == "00"
```

### Variable Definitions

* `I_cprstart` — Individual has valid Civil Registration Number start (DK: CPR-start)
* `f_dkstatus` — Residency status (DK: bopælsstatus)
* `is_dead` — Mortality indicator
* `f_cprsens` — CPR sensitivity flag (DK: CPR-følsomhed)

---

⚠ **Possible Problems**

* Definitions of residency exit (`"exit"`) may differ across cohorts.
* CPR sensitivity (`f_cprsens`) exclusions may bias specific subpopulations.
* Mortality timing relative to observation window may introduce misclassification.

✔ **Possible Solutions**

* Explicitly define residency eligibility criteria per study.
* Perform sensitivity analyses with and without CPR-sensitive individuals.
* Ensure time-aligned mortality filtering.

---

# Income

Income is derived from the **IND register** (DK: Indkomstregister).

Individuals are categorized into **population-level income quintiles**:

* 0–20%
* 20–40%
* 40–60%
* 60–80%
* 80–100%

### Important Notes

* Children are **not excluded**, and therefore constitute a substantial proportion of the lowest quintile.
* Missing income values may be allocated to the most common group (often 0–20%) — this must be verified.

---

⚠ **Possible Problems**

* Quintiles are defined at a general population level and may not represent true quintiles within specific study populations.
* Exclusion of specific subgroups (e.g., psychiatric history, children) can distort distribution.
* Allocation of missing values to the lowest quintile may introduce bias.

✔ **Possible Solutions**

* Define quintiles **within each study population**.
* Use continuous income ranking (e.g., percentile position such as `0.13` for 13th percentile).
* Use household income (DK: husstandsindkomst) from:
  * `IND`
  * `FAIK`
* Map family income from `FAIK` via `BEF` (population register, DK: Befolkningsregister).
* Develop a standardized quintile-generation function for internal use.

🕮 **Resources**

* [Documentation from Statistics Denmark (in Danish)](https://www.dst.dk/da/Statistik/dokumentation/statistikdokumentation/indkomststatistik)
* [Baadsgaard and Quitzau (2011): Danish registers on personal income and transfer payments](https://journals.sagepub.com/doi/10.1177/1403494811405098)
* [Ejlskov and Plana-Ripoll (2025): Income in epidemiological research: a guide to measurement and analytical treatment with a case study on mental disorders and mortality](https://pure.au.dk/portal/da/publications/income-in-epidemiological-research-a-guide-to-measurement-and-ana/)

---

# Education

Highest attained education level is derived from the **UDDF register** (DK: Uddannelsesregister).

Information on education are obtained from various sources of different quality. The proportion of different source types over time can be found here: https://www.dst.dk/da/TilSalg/Forskningsservice/Dokumentation/hoejkvalitetsvariable/hoejst-fuldfoerte-uddannelse/hf-kilde

Note that:

* All information on education obtained before 1970 are self-reported.

* Education for foreigners might be self-reported, based on previous qualifications needed to begin education or work in Denmark, or imputed.

The variable `hf_kilde` identifies imputed information (given by values 10,16,18,7,9).

The imputation is based on information that we do not have access to in the registers. DST provides details on the imputation in the following: https://www.dst.dk/Site/Dst/SingleFiles/GetArchiveFile.aspx?fi=formid&fo=medbragt-udd--pdf&ext=%7B2%7D

Education is categorized based on variable `hfaudd`.



| Group                          | hfaudd value | Mapping Description                           |
| ------------------------------ | --------- | --------------------------------------------- |
| No education                   | —         | No registered education                       |
| Primary school                 | —         | Primary education                             |
| Vocational training            | —         | Vocational education (DK: erhvervsuddannelse) |
| Gymnasium                      | —         | Upper secondary school                        |
| Higher education (short cycle) | —         | Short tertiary education                      |
| Higher education (long cycle)  | —         | Long tertiary education                       |

 A mapping can be found in ./03_keys/N_AUUD_HOVED_E_L5L5_KT.txt and ./03_keys/N_AUDD_HOVED_L1L5_KT.txt. 

*(Note: Vocational training and gymnasium may optionally be separated.)*

---

⚠ **Possible Problems**

* Older individuals and immigrants may have self-reported education.
* Immigrants (particularly pre-1970 migrants) have self-reported education.
* Missing education values for immigrants are imputed by DST. Imputed values can be a problem for prediction models and uncertainty measurements in statistical analyses.
* No explicit missing category exists.

✔ **Possible Solutions**

* Exclude individuals with imputed education.
* Create binary indicator for imputed education using `hf_kilde`.
* Stratify analyses by imputed vs non-imputed education.
* Separate vocational training and gymnasium into distinct categories.

🕮 **Resources**

* [Documentation from Statistics Denmark (in Danish)](https://www.dst.dk/da/Statistik/dokumentation/statistikdokumentation/hoejest-fuldfoert-uddannelse)
* [Jensen and Rasmussen (2011): Danish Education Registers](https://journals.sagepub.com/doi/10.1177/1403494810394715?url_ver=Z39.88-2003&rfr_id=ori:rid:crossref.org&rfr_dat=cr_pub%20%200pubmed)

---

# Employment / Work Status

Derived from:

* AKM (DK: Arbejdsmarkedsregister)
* RAS (DK: Registerbaseret Arbejdsstyrkestatistik)

Primary variable used: `SOCIO_13` (from AKM)

---

## Version Implemented (2024-12-18)

| Group              | SOCIO_13 Values | Mapping                                                                           |
| ------------------ | --------------- | --------------------------------------------------------------------------------- |
| Kids and Education | 310, 420        | Under education                                                                   |
| Employed           | 110–139, 410    | Employed, self-employed, others                                                   |
| Unemployed         | 210             | Unemployment benefits                                                             |
| Not in workforce   | 220, 330        | Cash benefits (DK: kontanthjælp), sick leave                                      |
| Retired            | 321–323         | Retired, early retirement (DK: efterløn), disability pension (DK: førtidspension) |

---

⚠ **Possible Problems**

* “Retired” includes both standard retirees and disability pension recipients.
* Individuals on cash benefits (DK: kontanthjælp) may resemble unemployed rather than retired.
* `SOCIO_13 = 410` (“Others, outside workforce”) may not fit the employed category.
* Missing values may be allocated to unemployed.

✔ **Possible Solutions**

* Exclude or impute missing values explicitly.
* Reclassify employment categories.


---

## Revised Version (Proposed)

| Group              | SOCIO_13 Values | Mapping                                                                              |
| ------------------ | --------------- | ------------------------------------------------------------------------------------ |
| Kids and Education | 310, 420        | Under education                                                                      |
| Employed           | 110–139, 410    | Employed, self-employed                                                              |
| Unemployed         | 210, 330        | Unemployment benefits, cash benefits (DK: kontanthjælp)                              |
| Not in workforce   | 220, 321, 323   | Disability pension (DK: førtidspension), early retirement (DK: efterløn), sick leave |
| Retired            | 322             | Standard retirement                                                                  |

---

## Open Classification Questions

* Should `SOCIO_13 = 410` (“Others outside workforce”, DK: øvrige ude af erhverv) be moved to “Not in workforce”?
* Should `SOCIO_13 = 323` (early retirement, DK: efterløn) be classified as “Retired”?


🕮 **Resources**

* [Documentation on RAS from Statistics Denmark (in Danish)](https://www.dst.dk/da/Statistik/dokumentation/statistikdokumentation/registerbaseret-arbejdsstyrkestatistik--ras-)
* [Documentation on AKM from Statistics Denmark (in Danish)](https://www.dst.dk/da/TilSalg/data-til-forskning/generelt-om-data/dokumentation-af-data/hoejkvalitetsvariable/personers-tilknytning-til-arbejdsmarkedet-set-over-hele-aaret--akm-)
* [Petersson, Baadsgaard and Thygesen (2011): Danish registers on personal labour market affiliation](https://journals.sagepub.com/doi/10.1177/1403494811408483?url_ver=Z39.88-2003&rfr_id=ori:rid:crossref.org&rfr_dat=cr_pub%20%200pubmed)

---

# Hospital diagnoses

Hospital diagnoses including both psychiatric and somatic diagnoses are available in Landspatientsregistret (LPR) which exits in several versions: 

* LPR2 (DK: Landspatientregisteret version 2)
* LPR3 (DK: Landspatientregisteret version 3)

The definition of contacts changes from LPR2 to LPR3. In LPR2, a contact is recorded at the level of a complete treatment sequence, which can be either
* an inpatient admission
* an outpatient treatment course
* an emergency department visit.

In LPR3 2019, a contact is recorded at the level of an element in a treatment sequence, e.g. an inpatient admission where the patient has three meetings with a psychiatrist is recorded as one contact in LPR2 and three contacts in LPR3. The number of contacts and diagnoses increases significantly after 2019. Several potential issues arises when combining data from LPR2 and LPR3, e.g. around the time of transition some contacts are artificially closed and reopened. 

---

⚠ **Possible Problems**

* Substantial differences exist between LPR2 and LPR3 coding practices. This particularly affects how contacts are defined.
* In LPR2, `c_adiag` should not be used — use `c_diag`.

✔ **Possible Solutions**

* Harmonize diagnosis definitions across LPR2 and LPR3.
* Conduct sensitivity analyses across register versions.
* A mapping of inpatient contacts across LPR2 and LPR3 can be found in [Mental health disorders before, during and after the COVID-19 pandemic: a nationwide study (Grønkjær et al., 2025)](https://academic.oup.com/brain/article/148/5/1829/7879585?login=true)

🕮 **Resources**

* [Documentation from Sundhedsdatastyrelsen (in Danish)](https://sundhedsdatastyrelsen.dk/data-og-registre/nationale-sundhedsregistre/landspatientregisteret)
* [Schmidt et al. (2015): The Danish National Patient Registry: a review of content, data quality, and research potential](https://pmc.ncbi.nlm.nih.gov/articles/PMC4655913/)

---

# Psychiatric Diagnoses 

Derived from:

* PCR (DK: Det Psykiatriske Centrale Forskningsregister)
* LPR2 (DK: Landspatientregisteret version 2)
* LPR3 (DK: Landspatientregisteret version 3)

Currently, only diagnoses given at psychiatric departments in LPR2 and LPR3 are considered. Adding diagnoses from neurological departments (or all somatic departments) might make sense, especially for organic mental disorders.

b1diag, b2diag, b3diag are coded as supplementary codes in PCR8. 

An increasing number of individuals receive a F-chapter diagnosis (especially ADHD and ASD diagnoses) as a tillægskode before receiving the diagnosis as a primary or secondary diagnosis. In most cases, the primary diagnosis will be a Z-code.

---

⚠ **Possible Problems**

* It might be more appropriate to classify b1diag, b2diag, b3diag in PCR8 as secondary diagnoses, cf: https://www.yumpu.com/da/document/read/17645037/variabelbeskrivelse-for-det-psykiatriske-centrale-forskningsregister/9#google_vignette.
* An increasing number of individuals receive a F-chapter diagnosis (especially ADHD and autism spectrum diagnoses) as a supplementary codes before receiving the diagnosis as a primary or secondary diagnosis. In most cases, the suplementray code specify a primary Z-code diagnosis.
* Substantial differences exist between LPR2 and LPR3 coding practices. This particularly affects how contacts are defined [(cf. Hospital Diagnoses)](#hospital-diagnoses).
* In LPR2, `c_adiag` should not be used — use `c_diag`.

✔ **Possible Solutions**

* Harmonize diagnosis definitions across LPR2 and LPR3.
* Explicitly document primary vs supplementary diagnosis strategy.
* Conduct sensitivity analyses across register versions.
* A mapping of inpatient contacts across LPR2 and LPR3 can be found in [Mental health disorders before, during and after the COVID-19 pandemic: a nationwide study (Grønkjær et al., 2025)](https://academic.oup.com/brain/article/148/5/1829/7879585?login=true)

🕮 **Resources**

* [Mors, Perto and Mortensen (2011): The Danish Psychiatric Central Research Register](https://journals.sagepub.com/doi/10.1177/1403494810395825?url_ver=Z39.88-2003&rfr_id=ori:rid:crossref.org&rfr_dat=cr_pub%20%200pubmed)
* [Plana-Ripoll et al. (2024): Mental Disorders in Danish Hospital Registers: A Review of Content and Possibilities for Epidemiological Research](https://www.dovepress.com/mental-disorders-in-danish-hospital-registers-a-review-of-content-and--peer-reviewed-fulltext-article-CLEP)
* [Bernstorff et al. (2022): Stability of diagnostic coding of psychiatric outpatient visits across the transition from the second to the third version of the Danish National Patient Registry](https://onlinelibrary.wiley.com/doi/10.1111/acps.13463)
* [Skjøth, Nielsen and Bodilsen (2022): Validity of Algorithm for Classification of In- and Outpatient Hospital Contacts in the Danish National Patient Registry](https://www.dovepress.com/validity-of-algorithm-for-classification-of-in--and-outpatient-hospita-peer-reviewed-fulltext-article-CLEP)

---

# Parental Psychiatric Disorder

---

⚠ **Possible Problems**

* Some fathers are not registered.
* Some registered fathers may not be biological fathers.
* Exposure definition varies depending on timing of parental diagnosis.
* Growing up with a parent with psychiatric illness differs from parental diagnosis occurring later in life.

✔ **Possible Solutions**

* Restrict to biologically verified parents where possible.
* Define exposure window explicitly (e.g., before age 18).
* Perform time-dependent exposure modelling.

---

# Charlson Comorbidity Index (CCI)

The Charlson Comorbidity Index (CCI) is used to estimate disease burden.

---

⚠ **Possible Problems**

* CCI is based on historical mortality weights.
* Some conditions (e.g., HIV) are now treatable and may be overweighted.
* In disease-specific studies (e.g., dementia), the outcome diagnosis should not be included in the index.

✔ **Possible Solutions**

* Review updated or alternative comorbidity indices.
* Modify weights for contemporary treatment contexts.
* Exclude outcome diagnosis from CCI calculation.
* Review alternative definitions (Andreas).

---

# Prescription Registry

Prescription data are available in Lægemiddelstatistikregistret (LMDB)

---

🕮 **Resources**

* [Documentation from Sundhedsdatastyrelsen (in Danish)](https://sundhedsdatastyrelsen.dk/data-og-registre/nationale-sundhedsregistre/laegemiddelstatistikregisteret)
* [Pottegaard et al. (2015): Data Resource Profile: The Danish National
Prescription Registry](https://www.antonpottegaard.dk/publications/reviews/o80.pdf)

---


# Laboratory results

The first available register on laboratory results was the regional Clinical Laboratory Information
System Research (LABKA) Database at Aarhus University. In the group, we have access to the more comprehensive nationwide Register of Laboratory Results for Research (RLRR). Colloquial (in our group), LABKA is however sometimes used to RLRR. 

---

🕮 **Resources**

* [Documentation from Sundhedsdatastyrelsen (in Danish)](https://sundhedsdatastyrelsen.dk/data-og-registre/nationale-sundhedsregistre/laboratoriedatabasen)
* [Håkonsen et al. (2020): Existing Data Sources in Clinical Epidemiology: Laboratory Information System Databases in Denmark](https://www.dovepress.com/article/download/53821)
