I have used two tables from Covid-19 Italy dataset from BigQuery data sets. Using that I've run 10 queries. 


Q1: Find he number of patients hospitalized in intensive care in Piemonte region each day.

```SQL
SELECT 
    date,
    region_name,
    hospitalized_patients_intensive_care
FROM 
    bigquery-public-data.covid19_italy.data_by_region
WHERE 
    region_name = 'Piemonte';
```
![Q1](https://github.com/geetashree10/geetashree10/assets/158750505/1a24156d-cd6d-404a-a9a0-810c3df12daa)


Q2: Find the number of patients that died on 2020-03-30 in Piemonte Region.

```SQL
SELECT deaths
FROM bigquery-public-data.covid19_italy.data_by_region
WHERE date = '2020-03-09 18:00:00' AND region_name = 'Piemonte';
```
![Q2](https://github.com/geetashree10/geetashree10/assets/158750505/ac567881-a00e-4bdc-87fe-f45cece7ac19)

Q3: Find the total number of tests performed from 21-07-2023 to 20-07-2023.

```SQL
SELECT SUM(tests_performed) AS total_tests
FROM bigquery-public-data.covid19_italy.data_by_region
WHERE date BETWEEN '2023-07-21 17:00:00' AND '2023-07-30 17:00:00';
```
![Screenshot 2024-02-04 163833](https://github.com/geetashree10/geetashree10/assets/158750505/cf4cefc6-fee4-4426-808e-9ae3748266ab)

Q4: Find the number of patients hospitalized in critical care out of the total number of hospitalized patients.

```SQL
SELECT 
    SUM(hospitalized_patients_intensive_care) AS intensive_care_patients,
    SUM(total_hospitalized_patients) AS total_hospitalized_patients
FROM bigquery-public-data.covid19_italy.data_by_region;
```
![Screenshot 2024-02-04 164004](https://github.com/geetashree10/geetashree10/assets/158750505/1a5f45e4-89fe-4411-9602-ed879918f0d9)

Q5: Find the total number of home confinement cases in Piemonte region of Italy.

```SQL
SELECT
    region_name,
    SUM(home_confinement_cases) AS total_home_confinement_cases
FROM
    bigquery-public-data.covid19_italy.data_by_region
WHERE
    region_name= 'Piemonte'
GROUP BY
    region_name;
```
![Screenshot 2024-02-04 164518](https://github.com/geetashree10/geetashree10/assets/158750505/09989c1c-3f61-440e-8a30-ec729627000d)

Q6: Find the total number of recovered cases in Italy.

```SQL
SELECT 
    SUM(recovered) AS total_recovered
FROM 
    bigquery-public-data.covid19_italy.national_trends
WHERE 
    country = 'ITA';
```

![Screenshot 2024-02-04 164637](https://github.com/geetashree10/geetashree10/assets/158750505/760ef8d9-9573-4fa2-8291-2ce754d4899e)


Q7: Find the total number of deaths in Italy in the month of November 2023.

```SQL
SELECT
    SUM(deaths) AS total_deaths
FROM
    bigquery-public-data.covid19_italy.national_trends
WHERE
    country = 'ITA'
    AND date BETWEEN TIMESTAMP('2023-11-01 17:00:00') AND TIMESTAMP('2023-11-30 17:00:00');
```
![Screenshot 2024-02-04 164916](https://github.com/geetashree10/geetashree10/assets/158750505/f9f9a634-20a0-4824-87c1-f81a4209eb2e)

Q8: Find the number of cases in Piemont vs total number of cases in Italy each day

```SQL
SELECT
    nt.date,
    nt.total_confirmed_cases AS national_total_confirmed_cases,
    dr.total_confirmed_cases AS piemonte_total_confirmed_cases
FROM
    `bigquery-public-data.covid19_italy.national_trends` nt
JOIN
    `bigquery-public-data.covid19_italy.data_by_region` dr
ON
    nt.date = dr.date
    AND nt.country = dr.country
WHERE
    nt.country = 'ITA'
    AND dr.region_name = 'Piemonte';
```
![Screenshot 2024-02-04 165024](https://github.com/geetashree10/geetashree10/assets/158750505/225c9c2a-739c-4074-ae71-64c48ea2d7f0)


Q9: Find the total number of deaths in Umbria and in Italy each day.

```SQL
SELECT
    t1.date,
    t1.country,
    t2.region_name,
    t1.deaths AS national_deaths,
    t2.deaths AS umbria_deaths
FROM
    bigquery-public-data.covid19_italy.national_trends t1
LEFT JOIN
    bigquery-public-data.covid19_italy.data_by_region t2
ON
    t1.date = t2.date AND t1.country = t2.country
WHERE
    t1.country = 'ITA' AND t2.region_name = 'Umbria';
```
![Screenshot 2024-02-04 165136](https://github.com/geetashree10/geetashree10/assets/158750505/9f03b7b6-f7ce-49ba-b0f6-e10ef3a613ef)

Q10: Find the total number of people recovered in Piemonte region as a percentage of national total.

```SQL
SELECT
    r.date,
    r.region_name,
    r.recovered AS region_recovered,
    n.recovered AS italy_recovered,
    (r.recovered / n.recovered * 100) AS percent_recovery
FROM
    bigquery-public-data.covid19_italy.data_by_region r
LEFT JOIN
    bigquery-public-data.covid19_italy.national_trends n
ON
    r.date = n.date
    AND r.country = n.country
WHERE
    r.country = 'ITA'
    AND r.region_name = 'Piemonte'
```

![Screenshot 2024-02-04 165248](https://github.com/geetashree10/geetashree10/assets/158750505/20116955-dc56-4ecd-a331-7b33fffb0874)




