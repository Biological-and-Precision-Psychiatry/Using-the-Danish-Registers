# Definition of study population and variables on the Statistics Denmark Registers

(work in progress)

Authors: [Clara S. Grønkjær](https://github.com/claragronkjar), 
[Eva N. S. Wandall](https://github.com/evaninawandall), 

Date started: 2024-12-18
Last edit: 2026-02-23

## Purpose of this document

This document is aimed for people who are going to start a
register-based study. It covers the definition of study population and
variables currently implemented on the registers. Also, it points out
things to be aware of and make decisions about, marked as:

<u>(!) Possible problems</u>

As well as their possible solutions, marked as:

<u>Possible solutions</u>

The topics covered are:


## Table of Contents

[Study population and flags](#study-population-and-flags)

[Income [3](#income)](#income)

[Education [4](#education)](#education)

[Employment/work status
[5](#employmentwork-status)](#employmentwork-status)

[Parental psychiatric disorder [6](#_Toc185432417)](#_Toc185432417)

[Charlson Comorbidity Index
[7](#charlson-comorbidity-index-cci)](#charlson-comorbidity-index-cci)

## Study population and flags

Flag i_valid (see 10_make_population.rmd) marks if the observation
period is valid.

i_valid =1:

I_cprstart == 1 and f_dkstatus != “exit” and is_dead == 0 and f_cprsens
== “00”

<u>(!) Possible problems:</u>

<u>Possible solutions:</u>

## Income

The personal income per individual is derived from the IND register. The
incomes are divided in quintiles on a population level:

- 0-20%

- 20-40%

- 40-60%

- 60-80%

- 80-100%

Note, kids are not excluded from this definition and therefore make up
x% of the 0-20% quintile. Also note, that missing values are allocated
to the most common group/0-20% group (?).

<u>(!) Possible problems:</u>

- The quintiles are not true quintiles in every study population, we
  make in this group. For example, when excluding individuals with
  pre-existing psychiatric disorders, we might exclude a lot of
  individuals in one quintile, thus skewing the variable. Another
  example is excluding kids, which makes the 0-20% group very small.

<u>Possible solutions:</u>

- We could consider using husstandsindkomst.(available in IND and FAIK)

- Use numerical/continuous variables instead of categorical, e.g., 0.13
  if 13% of individuals have lower income values than the individual.

- Quintiles should be defined per study population for each study and
  cannot be made on a general level. A function that does this should be
  made available to everyone in the group.

- Family income from FAIK mapped onto income from IND by BEF

## Education

The highest attained education level is derived from the UDDF register.

The following levels are currently used:
-	No education information
-	Mandatory school
-	High school or vocational training
-	Short higher education (Bsc-level)
-	Higher education  (Ms/Phd-level)

With nearly 5000 different educations, it does not make sense to make the table in the current document. A mapping can be found in ./03_keys/N_AUUD_HOVED_E_L5L5_KT.txt and ./03_keys/N_AUDD_HOVED_L1L5_KT.txt. **(ENSW: From SAS formater?)**

Information on education are obtained from various sources of different quality. 
The proportion of different source types over time can be found here: https://www.dst.dk/da/TilSalg/Forskningsservice/Dokumentation/hoejkvalitetsvariable/hoejst-fuldfoerte-uddannelse/hf-kilde

Note that 

1.	All information on education obtained before 1970 are self-reported. 

2.	Education for foreigners might be self-reported, based on previous 
qualifications needed to begin education or work in Denmark, or imputed.

Note that imputed values can be a problem for prediction models and uncertainty
measurements in statistical analyses.

The variable hf_kilde in UDDF can be used to identify imputed information (given 
by values 10,16,18,7,9).

The imputation is based on information that we do not have access to in the 
registers, and it seems like it does a reasonable job. DST provides details on 
the imputation in the following file: 

https://www.dst.dk/Site/Dst/SingleFiles/GetArchiveFile.aspx?fi=formid&fo=medbragt-udd--pdf&ext=%7B2%7D

<u>(!) Possible problems:</u>

- Values for older people and immigrants are highly based on
  self-reported data or is imputed by DST. Therefore, there are no
  missing values in this data.

- Education for immigrants (and Danes before 1970) are in some cases
  self-reported

- Missing values for immigrants are in some cases imputed (er markeret,i variabel )

<u>Possible solutions:</u>

- Exclude individuals with missing values.

- Impute individuals with missing values

- Create binary value marking imputed values

- Vocational training and high school could be divided in two
  categories.

## Employment/work status

Employment status is derived from the AKM and RAS registries. They are
divided in 5 groups based on the AKM\$SOCIO_13 and RAS\$ARBSTILL
variable:

|  |  |  |
|----|----|----|
| **VERSION IMPLEMENTED 2024-12-18** |  |  |
| **Group** | **SOCIO_13 value** | **SOCIO_13 mapping** |
| Kids and Education | 310, 420 | Under education |
| Employed | 110-139, 410 | Employed, self-employed, others |
| Unemployed | 210 | Unemployment benefits |
| Not in workforce | 220, 330 | Cash benefits (*kontakthjælp*), sick leaves, etc. |
| Retired | 321-323 | Retired, voluntary early retirement (efterløn), incapacity benefits (*førtidspension*) |

(!) Possible problems:

- Retired will have normal retired people and people who may have health
  or social problems, thus there is a mix of socio economic statusses in
  Not in Workforce

- Same idea as above applies to people on kontanthjælp. They may be
  considered people of less severe socioeconomic status, thus more
  similar to unemployed.

- Cash benefits (*kontakthjælp*) is allocated to the Retired group but
  are potentially able to resume work.

- Missing values are allocated to the Unemployed group (?).

Possible solutions:

- Exclude individuals with missing values

- Impute missing values

- New definition of employment groups:

|  |  |  |
|----|----|----|
| **REVISED VERSION** |  |  |
| **Group** | **SOCIO_13 value** | **SOCIO_13 mapping** |
| Kids and Education | 310, 420 | Under education |
| Employed | 110-139, 410 | Employed, self-employed, others |
| Unemployed | 210, 330 | Unemployment benefits, **<span class="mark">cash benefits (*kontakthjælp*)</span>** |
| Not in workforce | 220,321,323 | **<span class="mark">Incapacity benefits</span> (*førtidspension*)**, voluntary early retirement (efterløn), sick leaves, etc. |
| Retired | 322 | Retired |

**ENSW** **(suggestions):**

410: Others. Not in workforce instead of employed? (fra beskrivelsen af
SOCIO13: ”Øvrige ude af erhverv( kode 410) ”)

323: Efterløn. Retired instead of not in workforce?



## Psykiatriske diagnoser

(!) Potentielle problemer:

The psych_diags_all file only contains diagnoses given at psychiatric departments.
We could consider adding diagnoses from neurological departments (or all somatic departments). Especially relevant for organic mental disorders.

b1diag, b2diag, b3diag are coded as "tillægskoder" for PCR8. **ENSW: Maybe it is more appropriate to consider them as secondary diagnoses: https://www.yumpu.com/da/document/read/17645037/variabelbeskrivelse-for-det-psykiatriske-centrale-forskningsregister/9#google_vignette.**

An increasing number of individuals receive a F-chapter diagnosis (especially 
ADHD and ASD diagnoses) as a tillægskode before receiving the diagnosis as a primary or
secondary diagnosis. In most cases, the primary diagnosis will be a Z-code.

The definition of contacts changes from LPR2 to LPR3. In LPR2, a contact is recorded at the level of a complete treatment sequence, which can be either

1. an inpatient admission 
2. an outpatient treatment course
3. an emergency department visit. 

In LPR3 2019, a contact is recorded at the level of an element in a treatment sequence, e.g. an inpatient admission where the patient has three meetings with a psychiatrist is recorded as one contact in LPR2 and three contacts in LPR3. The number of contacts and diagnoses increases significantly after 2019. Several potential issues arises when combining data from LPR2 and LPR3, e.g. around the time of transition some contacts are artificially closed and reopened. A mapping of inpatient contacts can be found in *Mental health disorders before, during and after the COVID-19 pandemic: a nationwide study* (Grønkjær et al., 2025)

LPR2: Don't use c_adiag, use instead c_diag (Why?)

LPR2:

## Parental psychiatric disorder

(!) Possible problems:

Some fathers are not registred and some fathers are not the real ones.

Please note that there is a difference between growing up with parents who have a mental illness and parents who are diagnosed later in life.

Possible solutions:

## Charlson Comorbidity Index (CCI)

(!) Possible problems:

Charlson Comorbidity Index is an old instrument which does not estimate
disease burden with state-of-the-art treatments available. Today for
example HIV is treatable and should probably not be weighted as high as
cancer.

In a Dementia study, the diagnosis should not be included in a
comorbidity index.

Possible solutions:

Review alternative definitions (Andreas)
