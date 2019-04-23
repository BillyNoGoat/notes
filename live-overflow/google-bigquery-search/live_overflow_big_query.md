https://www.youtube.com/watch?v=Xml4Gx3huag

###Cloud platform
One of the services Google offers is BigQuery which is a fast, eceonomical and fully-managed enterprise data warehouse for large-scale data analytics.

It can scan terabytes in seconds and petabytes in minutes.

You write an SQL query for a big data set. It comes with public data sets such as baseball games, census data, crimes etc. There are some cool examples to play with or you can use the data for a research project.

There is a public github data set which we can use to search all the data from every github repo.

I wrote a regular expression for IOTA addresses which are 81 character long strings of capital letters A-Z and 9's which looks like: `[A-Z9]{81}`

I then wrapped this around two non capturing groups to ensure it's not matched in a longer string so we are checking for the start or end of a string of anything that is not a regular character. Our final expression is:
`(?:^|[^a-zA-Z0-9])[A-Z9]{81}(?:$|[^a-zA-Z0-9])`

Now we have our expression, we can go to bigquery.cloud.google.com and run a query to search all github repo's and find strings that match our IOTA key string.

We can see public data is in the dataset `bigquery-public-data`. We can then look at the structure of the table in the UI to understand which fields etc we need to account for.

Now we have our regex, we have our data set and table/field information, we can start to write a SQL query to get all the data.

```SELECT
  f.repo_name,
  f.path,
  c.pkey
FROM
  [bigquery-public-data:github_repos.files] f
JOIN (
  SELECT
    id,
    REGEXP_EXTRACT(content, r'(?:^|[^a-zA-Z0-9])([A-Z9]{81})(?:$|[^a-zA-Z0-9])') AS pkey
  FROM
    [bigquery-public-data:github_repos.contents]
  WHERE
    REGEXP_MATCH(content, r'(?:^|[^a-zA-Z0-9])([A-Z9]{81})(?:$|[^a-zA-Z0-9])') ) c
ON
  f.id = c.id
```
To break this down, we first query all file contents where the regex matches and return those matches together with an id. We then join that result with the files database which contains the repository name and the file name/path. The result will be a table with the keys and where they are from.

Now we have our data set output, we can export that. If the file is too big, we will need to save this to a table in the google cloud platform.

To do that, you can scroll to the top of your project and create a new empty data set. Now when we click "save as table" we can save to that data set with a new table containing the information. We can then also export that to our google cloud bucket by clicking the down arrow and clicking export and then changing the "Google Cloud Storage URI" to `gs://[BUCKET_NAME]/[FILENAME.CSV]`. A new bucket can be created here: console.cloud.google.com/storage.
