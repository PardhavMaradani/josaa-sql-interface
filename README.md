# JoSAA SQL Interface

This is a browser based **SQL Interface** for JoSAA Opening and Closing Rank [data](https://josaa.nic.in/or-cr/), which allows you to explore and analyze using **SQL** queries.

## Analyze

- Click [here](https://pardhavmaradani.github.io/josaa-sql-interface/) to analyze this data in your browser
  - The Anaysis URL is https://pardhavmaradani.github.io/josaa-sql-interface/
  - > Note that for the very first time, it could take a few seconds / minutes to download the database and set it up depending on your internet speed (The database file is ~19MB)
  - All queries work locally in your browser after that
- The web interface should look something like this:

![JoSAA SQL Web Interface](images/josaa-sql-interface-sample1.png)

- Enter your **SQL** commands in the textarea and click on the `Execute` button or `Ctrl-Enter` to see the results.

![JoSAA SQL Web Interface Example](images/josaa-sql-interface-sample2.png)

- The `db/josaa-2016-2024-all.db` file can be downloaded and analyzed locally using `sqlite` as well
- All the CSV's of the data are available separately in the `csv` folder and can be analyzed in any spreadsheet tool

## About the data

- The database consists of Opening and Closing rank details of all institutes (IITs, NITs, IIITs and GFTIs) that take part in JoSAA for all academic programs and all categories for the years shown below:

  - 2024 - rounds 1 through 5 (final)
  - 2023 - final round 6
  - 2022 - final round 6
  - 2021 - final round 6
  - 2020 - final round 6
  - 2019 - final round 7
  - 2018 - final round 7
  - 2017 - final round 7
  - 2016 - final round 6

- All the CSV files used for creating the DB were exported using the [josaa-export-csv](https://github.com/PardhavMaradani/josaa-export-csv) Chrome Browser Extension.

- The DB file was created as shown below:

```
$ sqlite3
SQLite version 3.46.0 2024-05-23 13:25:27
Enter ".help" for usage hints.
Connected to a transient in-memory database.
Use ".open FILENAME" to reopen on a persistent database.
sqlite> .mode csv
sqlite> create table data(year TEXT, round TEXT, type TEXT, institute TEXT, program TEXT, quota TEXT, category TEXT, gender TEXT, orank NUMBER, crank NUMBER);
sqlite> .import --skip 1 josaa-2024-r1-all.csv data
sqlite> .import --skip 1 josaa-2024-r2-all.csv data
sqlite> .import --skip 1 josaa-2024-r3-all.csv data
sqlite> .import --skip 1 josaa-2024-r4-all.csv data
sqlite> .import --skip 1 josaa-2024-r5-all.csv data
sqlite> 
sqlite> .import --skip 1 josaa-2023-r6-all.csv data
sqlite> .import --skip 1 josaa-2022-r6-all.csv data
sqlite> .import --skip 1 josaa-2021-r6-all.csv data
sqlite> .import --skip 1 josaa-2020-r6-all.csv data
sqlite> .import --skip 1 josaa-2019-r7-all.csv data
sqlite> .import --skip 1 josaa-2018-r7-all.csv data
sqlite> .import --skip 1 josaa-2017-r7-all.csv data
sqlite> .import --skip 1 josaa-2016-r6-all.csv data
sqlite> select count(*) from data;
118581
sqlite> .save ../db/josaa-2016-2024-all.db
sqlite> 

$
```

- [sql.js](https://github.com/sql-js/sql.js) was used to create the SQL Web Interface

## Running locally

- To run a local version:
  - Clone this repo
  - Start a local server in the cloned folder using: `$ ./start_local_server.py`
  - Open `http://localhost:8081/` in your browser

## Writing / Generating SQL Queries

Here is the `schema` of the single table in this SQLite database:

```
CREATE TABLE data(year TEXT, round TEXT, type TEXT, institute TEXT, program TEXT, quota TEXT, category TEXT, gender TEXT, orank NUMBER, crank NUMBER);
```

Here are the first few rows of this table:

```
SELECT * FROM data LIMIT 5;
```

```
┌──────┬───────┬──────┬────────────────────────────────────────────┬─────────────────────────────────────────────────────┬───────┬──────────┬────────────────┬───────┬───────┐
│ year │ round │ type │                 institute                  │                       program                       │ quota │ category │     gender     │ orank │ crank │
├──────┼───────┼──────┼────────────────────────────────────────────┼─────────────────────────────────────────────────────┼───────┼──────────┼────────────────┼───────┼───────┤
│ 2024 │ 1     │ IIT  │ Indian Institute of Technology Bhubaneswar │ Civil Engineering (4 Years, Bachelor of Technology) │ AI    │ OPEN     │ Gender-Neutral │ 9106  │ 13018 │
│ 2024 │ 1     │ IIT  │ Indian Institute of Technology Bhubaneswar │ Civil Engineering (4 Years, Bachelor of Technology) │ AI    │ OPEN     │ Female-only    │ 18286 │ 20788 │
│ 2024 │ 1     │ IIT  │ Indian Institute of Technology Bhubaneswar │ Civil Engineering (4 Years, Bachelor of Technology) │ AI    │ EWS      │ Gender-Neutral │ 1755  │ 1975  │
│ 2024 │ 1     │ IIT  │ Indian Institute of Technology Bhubaneswar │ Civil Engineering (4 Years, Bachelor of Technology) │ AI    │ EWS      │ Female-only    │ 3122  │ 3308  │
│ 2024 │ 1     │ IIT  │ Indian Institute of Technology Bhubaneswar │ Civil Engineering (4 Years, Bachelor of Technology) │ AI    │ OBC-NCL  │ Gender-Neutral │ 3573  │ 4796  │
└──────┴───────┴──────┴────────────────────────────────────────────┴─────────────────────────────────────────────────────┴───────┴──────────┴────────────────┴───────┴───────┘
```

Any SQL query that works against an SQLite database should work.  For those unfamiliar with SQL, an option would be to use [ChatGPT](https://chatgpt.com) to generate SQL queries as described in the [neet-2024-center-marks](https://github.com/PardhavMaradani/neet-2024-center-marks) repo.
