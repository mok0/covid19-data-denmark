# Epidemiological data of COVID-19 for Denmark

This archive contains data from [Statens Serum Institut (SSI)][1], an
institution under the auspices of the Danish Ministry of Health.

Most of the data in this repo has been extracted by me by scraping the
official COVID-19 status web page of the [Danish Ministry of
Health][2]. Although the the data is also available from the producer
[Statens Serum Insitut][3] I found that the former web page changes far
less frequently.

June 17, 2020 SSI changed the definition of COVID-19 related
hospitalizations to be defined as hospitalization occurring within 14
days after the first positive test. Furthermore, patients still
hospitalized 90 days after first positive tests are removed from the
statistic. The numbers for hospitalizations (+ ICU, ventilators) have
been updated accordingly.

## Content CSV Data Files

### covid19-data-denmark.csv

The data contained in the file `covid19-data-denmark.csv` includes the
following columns:

- `day`: days since Monday 2020-02-24, the first day of the week where
  the first COVID-19 cases were first recorded in Denmark.
- `date`: The date in ISO 8601 format.
- `tests_accum`: Accumulated number of performed tests. This data was
   not provided before 2020-05-15.
- `tests_today`: Tests reported on this date. This data was not
   provided before 2020-05-16.
- `tested_accum`: Accumulated number of tested persons by PCR tested
  one or more times. Daily updates to this data was not provided
  before 2020-03-02, the total of all tested persons before that (358)
  are recorded on this day.
- `tested_today`: Persons PCR tested on this day that have not been tested before.
- `infected_accum`: Accumulated number of confirmed cases by PCR test.
- `infected_today`: Number of infected patients confirmed by PCR test on this date.
- `dead_accum`: Accumulated number of deaths associated with the
  COVID-19 pandemic.
- `dead_today`: Number of persons reported dead on this date.
- `recovered_accum`: Accumulated number of recoveries after positive PCR result.
- `recovered_today`: Persons reported recovered on this date after positive PCR result.
- `active`: Active cases. Computed per day using this formula:
  ```infected_accum - recovered_accum - dead_accum```
- `hospitalized_now`: Number of patients currently hospitalized.
- `hospitalized_change`: Change in hospitalizations since the day before.
- `hospitalized_admitted`: Newly admitted patients. (*)
- `icu_now`: Number of patients currently in Intensive Care Units.
- `icu_change`: Change of ICU patients since the day before.
- `respirator_now`: Number of patients currently in ventilators.
- `respirator_change`: Change in number of patients in ventilators since
  the day before.

(*) The data in the `hospitalized_admitted` column is extracted from
the dataset `Newly_admitted_over_time.csv` that can be found in a zip-archive
updated on weekdays and published on [SSIs web page][3].

All other data is scraped from SSI's [COVID-19 dashboard][covid-19-dashboard].


Note1: 2021-03-10: The data from the date 2021-03-10 contains errors
in the numbers. On this date, SSI began pooling antigen and PCR data
from private test centres to the official ones, and apparently an
error was made. This is _extremely_ bad, since there are many
consumers of the data published by SSI, and that data is now tainted,
since we don't know how many patients appear two or more times in the
statistics.

Note2: 2021-03-26. From this date, SSI has again revised the data
reporting on their [COVID-19 dashboard][covid-19-dashboard]. Antigen
tests and PCR tests are again separated. This means that data in the
period 2021-03-10 (day 380) to 2021-03-25 (day 395) contained results
from PCR + antigen tests, where many of the latter were false
positives. Using various sources, including data scraped from [this page][tal-og-overvaagning],
where PCR and Ag test numbers were given, the data in
`covid19-data-denmark.csv` has been corrected so only PCR data is
reported. The problem with antigen tests is that they are not
conclusive, and patients are referred to an PCR tests to give the
final answer. Corrections include the following:

- `infected_accum` is now data from smitteapp.infected_accum for
  `day >= 380 and day < 396`. The column `infected_today` is computed
  from this as daily difference.
- `tests_accum` contains data copied from the [SST webpage][tal-og-overvaagning]
  as described above, and `tests_today` is computed from this as a daily
  difference.
- The data in the columns `recovered_accum` and `tested_accum` is only
  reported on SSI's [COVID-19 dashboard][covid-19-dashboard], so a
  correction procedure was carried out. Using the PCR-only values of
  days 379 and 396, the true increase could be computed, and the data
  was scaled proportionally to fit these constraints. This procedure
  has removed data from the antigen tests, and further the huge
  positive and negative spikes in the data resulting from the two
  changes in data reporting practice by SSI. However data in the columns
  `recovered_accum`, `recovered_today`, `tested_accum` and
  `tested_today` should be considered *approximate* in the
  range day 380 to day 395 as they are not the exact recorded numbers.

Note3: 2021-06-29. From this date, SSI no longer provides the data on
first time tests, given in the fields `tested_today` and
`tested_accum`.


### vaccinations.csv

This file contains data of COVID-19 vaccinations, extracted from a PDF
file published by SSI on weekdays.

- `day`: days since Monday 2020-02-24, the first day of the week where
  the first COVID-19 cases were first recorded in Denmark.
- `date`: The date in ISO 8601 format.
- `week`: The ISO week number.
- `vaccination1_accum`: Accumulated number of first vaccination
- `vaccination1_today`: Today's number of first vaccination
- `vaccination2_accum`: Accumulated number of second vaccination
- `vaccination2_today`: Today's number of second vaccination

From the date 2021-02-22 SSI is publishing daily vaccination numbers
on their [vaccination dashboard][vacc-dashboard], rather than from at
table in a daily PDF file. It means yesterday's numbers are presented
as today's; therefore there is no data for day 363 (2021-02-21).

### private_testing.csv

This file contains data from private testing providers, extracted
from a PDF file published by SSI on weekdays. It is not known how many
of these patients also have been tested in the public testing system.
Indeed it is quite likely that patients with positive antigen test
have their results confirmed by taking a test in the public system,
which is free. Therefore, the data in this table cannot be added to
the data in the file `covid19-data-denmark.csv`.

- `day`: days since Monday 2020-02-24, the first day of the week where
  the first COVID-19 cases were first recorded in Denmark.
- `date`: The date in ISO 8601 format.
- `week`: The ISO week number.
- `tests_pcr_accum`: Performed PCR tests, accumulated
- `tests_pcr_today`: Performed PCR tests, today
- `infected_pcr_accum`: Infected patients found by PCR testing, accumulated
- `infected_pcr_today`: Infected patients found by PCR testing, today
- `tests_ag_accum`: Performed antigen tests, accumulated
- `tests_ag_today`: Performed antigen tests, today
- `infected_ag_accum`: Infected patients found by antigen testing, accumulated
- `infected_ag_today`: Infected patients found by antigen testing, today

Note: This data is not published after 2021-03-10. After this date,
the numbers implicitly appear in `covid19-data-denmark.csv`. Extremely
poor data reporting practice, but what can we do?

### mutant_data.csv

This file contains sequencing data on positive SARS-CoV-2 samples. The
data is extracted from the statistics page of the [Danish Covid-19 Genome Consortium](https://www.covid19genomics.dk).
The data is aggregated per week.

- `day`: The day number of the last day of the week (Sunday). Day zero
        is Monday 2020-02-24, the first day of the week where the
        first COVID-19 cases were first recorded in Denmark.
- `year`: The year
- `week`: Week number of the year.
- `variant`: The genome variant.
- `ireg`: The region number.
- `region`: The region name
- `positive`: Number of variant genomes sequenced
- `genomes`: Number samples sequenced
- `cases`: Total number of cases in that region and that week

## Log of Registered Changes to the Source Web Page

Table of registered changes to the [source web page][2]. Only events
that broke web scraping is recorded. The list of events before
2020-06-26 is not exhaustive.

| Date       | What                                                                                                       |
| :--------- | :----------------------------------------------------------------------------                              |
| 2020-04-02 | Official recovery stats available                                                                          |
| 2020-04-10 | Deal with date typo 10 .april                                                                              |
| 2020-05-16 | More exhaustive data provided in Table 1.1. New fields in tables 1.1 and 1.2                               |
| 2020-05-20 | Section 1.1 title changed                                                                                  |
| 2020-05-21 | Table 1.2 changed                                                                                          |
| 2020-05-12 | SSI removes 6 wrongly categorised patients from deaths count                                               |
| 2020-05-25 | Deal with date typo '24. ma' instead of '24. maj'                                                          |
| 2020-06-08 | Deal with date typo 'juní' instead of 'juni'                                                               |
| 2020-06-18 | Deal with typo in number of `recovered_accum` (12242 -> 11242) that made number of actives become negative |
| 2020-06-20 | Data is not updated on 2020-06-20 and 2020-06-21 due to server maintenance.                                |
| 2020-06-22 | Data collected during the weekend is reported on this date.                                                |
| 2020-06-26 | Section 1.1, 2.4 and 3.7 titles changed.                                                                   |
| 2020-06-27 | From this date, data is no longer updated on weekends.                                                     |
| 2020-07-21 | Section 3.7 title changed.                                                                                 |
| 2020-08-24 | Scrape data from [SSI's webpage] which now includes weekend data
| 2020-08-31 | Section 3.7 title changed.                                                                                 |
| 2020-10-26 | Accumulated tests data for day 0 to 81 added from SSI's file `Test_pos_over_time.csv`. |
| 2020-10-27 | Between 2020-06-19 and 2020-08-24 data was reported collectively on mondays, weekend data in this range interpolated. |
| 2021-01-12 | Added data files `vaccinations.csv` and `private_testing.csv` |
| 2021-02-08 | Added data file `mutant_data.csv` |
| 2021-02-22 | SSI now published daily vaccination numbers on their vaccination dashboard |
| 2021-03-10 | From this date, SSI adds testing data from private test centres, antigen and PCR are pooled. This is very bad. |
| 2021-03-25 | SSI have again changed their data reporting, now antigen tests are no longer included, so this day's numbers become negative if you check. Extremely bad. |
| 2021-03-26 | Data has been corrected to exclude antigen test data, see notes 1 & 2 above |

## License

<p xmlns:dct="http://purl.org/dc/terms/"
  xmlns:vcard="http://www.w3.org/2001/vcard-rdf/3.0#"> <a
  rel="license"
  href="http://creativecommons.org/publicdomain/zero/1.0/"> <img
  src="http://i.creativecommons.org/p/zero/1.0/88x31.png"
  style="border-style: none;" alt="CC0" /> </a> <br /> To the extent
  possible under law, <a rel="dct:publisher"
  href="https://github.com/mok0/covid19-data-denmark"> <span
  property="dct:title">Morten Kjeldgaard</span></a> has waived all
  copyright and related or neighboring rights to <span
  property="dct:title">Official COVID-19 data for Denmark</span>. This
  work is published from: <span property="vcard:Country"
  datatype="dct:ISO3166" content="DK"
  about="https://github.com/mok0/covid19-data-denmark">
  Denmark</span>.
</p>


[1]: https://en.ssi.dk/about-us
[2]: https://www.sst.dk/da/corona/tal-og-overvaagning
[3]: https://www.ssi.dk/sygdomme-beredskab-og-forskning/sygdomsovervaagning/c/covid19-overvaagning
[covid-19-dashboard]: https://experience.arcgis.com/experience/aa41b29149f24e20a4007a0c4e13db1d/page/page_0/
[vacc-dashboard]: https://experience.arcgis.com/experience/1c7ff08f6cef4e2784df7532d16312f1
[tal-og-overvaagning]: https://www.sst.dk/da/corona/Status-for-epidemien/tal-og-overvaagning
