**Definition of study population and variables  
on the Statistics Denmark Registers**

(work in progress)

Author: Clara Springhorn Grønkjær

Date: 2024-12-18

**Purpose of this document**

This document is aimed for people who are going to start a
register-based study. It covers the definition of study population and
variables currently implemented on the registers. Also, it points out
things to be aware of and make decisions about, marked as:

<u>(!) Possible problems</u>

As well as their possible solutions, marked as:

<u>Possible solutions</u>

The topics covered are:

[Study population and flags
[2](#study-population-and-flags)](#study-population-and-flags)

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

The highest attained education level is derived from the UDDA register.
They are divided based on the XXX variable

|  |  |  |
|----|----|----|
| **Group** | **XXX value** | **XXX mapping** |
| No education |  |  |
| Primary school |  |  |
| Vocational training or gymnasium |  | <span class="mark">Vocational training and high school could be divided in two categories</span> |
| Higher education, short cycle |  |  |
| Higher education, long cycle |  |  |

<u>(!) Possible problems:</u>

- Values for older people and immigrants are highly based on
  self-reported data or is imputed by DST. Therefore, there are no
  missing values in this data.

- Uddannelsesniveau for indvandrere (danskere før 1970) er
  selvrapporteret

- Missing værdier for indvandrere er imputeret (er markeret,i variabel )

<u>Possible solutions:</u>

- Exclude individuals with missing values.

- Impute individuals with missing values

- Lav binær om uddannelsesnivueau er imputeret fra variable UDDF
  (Hvilken variabel Eva)

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

- Cash benefits (k*ontakthjælp*) is allocated to the Retired group but
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

<span id="_Toc185432417" class="anchor"></span>Psykiatriske diagnoser

## 

## Potentielle problemer:

## Især for Autisme of ADHD diagnoser kan ligge som tillægskoder (i LPR2 og før) og ikke som primær diagnose som kunne være en Z-kode (Jonas)

Stor forskel i hvordan psykiatriske diagnoser er rapporteret i LPR2 og
LPR3

LPR2: Brug ikke C_adiag, men brug C_diag

LPR2:

## Parental psychiatric disorder

(!) Possible problems:

Nogle fædre er ikke registret og nogle fædre er ikke de rigtige.

Vær opmærksom på at der er forskel på at vokse op med forældre med
psykisk sygdom og hvis forældre får diagnosen senere i livet.

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
