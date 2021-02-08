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
- `tested_accum`: Accumulated number of tested persons. Daily updates
  to this data was not provided before 2020-03-02, the total of all
  tested persons before that (358) are recorded on this day.
- `tested_today`: Persons tested on this day.
- `infected_accum`: Accumulated number of confirmed cases.
- `infected_today`: Number of infected patients confirmed on this date.
- `dead_accum`: Accumulated number of deaths associated with the
  COVID-19 pandemic.
- `dead_today`: Number of persons reported dead on this date.
- `recovered_accum`: Accumulated number of recoveries.
- `recovered_today`: Persons reported recovered on this date.
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
