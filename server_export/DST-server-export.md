DST-server export
================

# This is a DRAFT document

In this guideline we seek to describe how we export analysis results
from the DST servers.

The guideline is primarily for researchers working through our access to
the DST registers. You sometimes need to send home analysis results from
the servers to advance your research project and you don’t have the
rights to send home results yourself.

# Guidelines

- All server exports (DK: hjemsendelser) are handled by Clara Grønkjær
  and Rune Haubo Christensen
- Write an email to <datamanagement-email>; not to Clara or Rune. If you
  mail directly to Clara or Rune, you will be asked to send to
  <datamanagement-email>. The reason is that all relevant people have
  access to that mail and that all relevant correspondences are saved on
  that mail account irrespective of who are part of the team at any
  given time.
- Time your need for exports with our “office hours”. We don’t want to
  be continuously on call for server exports, so we consider server
  exports weekly on xxxdays afternoon. If you have a special need for
  exports outside our standard office hours reach out to us and we will
  come to an arrangement.
- Your email with a server export request need to contain the full path
  to the folder that holds the files that constitute your export
  request. Collect all files in a single folder. Place your folder under
  a path that resembles `Workdata/70XXXX/INIT/hjemsendelser/YYYY-MM-DD`
  where
  - `XXXX` is the relevant project (often `8237`)
  - `INIT` are your initials
  - `YYYY-MM-DD` are a recent date (often today)
- Every new server export request should be placed in a parallel folder
  (under `Workdata/70XXXX/INIT/hjemsendelser`) with a new (non-future)
  date.
- Always use manual copy-paste (ctrl-c; ctrl-v) to put files into the
  `hjemsendelser/YYYY-MM-DD`-folders; never use a program to copy-paste
  files into this folder
- Do your best to limit the need for manual review of materials that you
  request exported. This limits delays in exporting and reduces the time
  we need to spend.
- It is your job (not ours) as requester of an export to demonstrate and
  guarantee that the requested export confirms with this guideline and
  the requirements of DST and Sundhedsdatastyrelsen.
- Your email needs to explicitly say that you checked all files for
  micro data or other violations and that you guarantee that the results
  can be validly exported.
- Check that your export conforms with the requirements of DST \[link
  here\] and Sundhedsdatastyrelsen \[another link here\]. We require
  that all exports conform with both sets of requirements. For example,
  even though DST only requires that there be at least 3 individuals per
  mean value or “cell in a table”, the equivalent requirement from
  sundhedsdatastyrelsen is 5 individuals. Therefore we require that any
  results be based on at least 5 individuals.
- Exports of results from the servers should only in rare and
  exceptional cases contain anything other than rectangular
  tables/dataset in the form of Excel files (`*.xlsx` or `*.xls`) or CSV
  files `*.csv`.
- Check your export request tables using our **R** check functions:
  `Workdata/708237/RHBC/001_functions/check_functions.R` and make sure
  that they don’t find any small counts or similar. We apply the same
  functions when we check your export request and we will ask you to
  modify and clarify anything suspicious, which will only delays the
  data export.
  - Also eliminate any false negatives because any false negative
    requires time consuming manual review and human interaction. You may
    for instance have an exposure variable `exposure` with levels `0`,
    `1` etc. This is not micro data and *can* be exported, but it *will*
    show up as a false negative from the check functions. It is also
    easily eliminated by defining the levels of the exposure variable as
    `a`, `b` (or, though not as good, as `level0`, `level1`) etc.
- But I want to export a plot that I made on the register! The right way
  to do this is to export the relevant non-micro data in a `. csv` or
  `. xlsx` file and then build the relevant graphics outside the DST
  server (on a local computer). That way it is easy to see what data are
  exported and it is easy to demonstrate that the exported data are not
  micro data.

## Small number of subjects

- The official requirement is at least 5 individuals per result
- Results from the registers are only meaningful when based on rather
  large quantities of data, so if you are contemplating results based on
  few individuals you are on the wrong path. Any results based on less
  than 10 or 20 individuals are very rarely meaningful
- Use modelling such as smoothing splines to reduce the sensitivity
  toward results from small groups of individuals.

## Various thoughts

- Tal bør hjemsendes i regneark så de kan tjekkes på sædvanlig vis; ikke
  i pdf’er - I det her tilfælde bør tallene nok slet ikke hjemsendes.
- ROC og Calibration kurver bør smoothes fx med smoothing splines så
  enkelte datapunkter ikke optræder som diskrete jumps i kurverne. Det
  gjorde fx Sara i hendes projekter som også omhandlede prædiktion af
  selvmord. Det er sådan set det samme som vi gør i forhold til
  Kaplan-Meier kurver: de skal også smoothes før hjemsendelse fordi hver
  enkelt jump risikerer at beskrive et enkelt individ.
- En mulighed er at der for hvert performance mål fittes en smoothing
  spline som funktion af threshold og at en sådan tabel kan hjemsendes.
  Grafer bør i udgangspunktet plottes på sådanne hjemsendte data (selvom
  vi kan være venlige og hjemsende grafer enkelte gange, og i så fald
  helst pdf (dvs vektor-grafik; ikke pixel-grafik))
- Vi vil gerne undgå at hjemsende flere versioner af samme plots (her fx
  pdf og jpg). Om en uge kommer vedkommende måske tilbage og skal lige
  rette en stavefejl i titlen eller opdatere legend eller lign. Det er
  derfor altid bedst i udgangspunktet at hjemsende data der ligger til
  grund for plottet (evt. efter smoothing) så alt grafisk
  postprocessering kan foregå på brugerens lokale pc.

## From Clara’s meeting notes

Hjemsend så vidt muligt tabeller og ikke figurer, da

1.  tabeller er nemmere at tjekke for mikrodata end figurer, og
2.  så behøves man ikke hjemsende nye figurer, hvis format (tekst,
    farver osv.) skal ændres på figuren.

- (Data til) ikke-parametriske overlevelseskurver skal smoothes, så man
  er sikker på, at
  1.  ‘knæk’ på grafen ikke repræsenterer mikrodata, og
  2.  at grafen ikke plottes i et område, hvor der er for få individer.
- Det kunne også være sådan noget såsom kolonnenavnekonventioner, som at
  kolonner med antal individer skal være ‘N’.

## Remember to

- [ ] Check the on-boarding document for things that should be reflected
  here.

## Consider to

- [ ] Review the DST guidelines for things that we need to mention here
- [ ] Review the guidelines from Sundhedsdatastyrelsen for things that
  we need to mention here
- [ ] Review the guideline from NCRR for things that we need to mention
  here

# Flow diagram

Pending input from Lars…
