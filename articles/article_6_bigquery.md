# My BigQuery Python & Costs Adventure

## TL;DR

In my quest to enhance my profile with PyPi download counts, I ventured into BigQuery, only to face unexpected costs. 
The journey involved crafting SQL queries, implementing a Lambda function for API exposure, and optimizing for performance. 
Initial challenges led to an EventBridge Scheduler upgrade, and a reality check prompted adjustments to minimize costs. 
An "Oh No!" moment revealed the impact of extensive data processing on expenses. 
Swift collaboration with Google billing support resolved financial concerns, 
leaving me with a valuable lesson: while BigQuery is user-friendly, beware of potential cost pitfalls in the cloud.

## The Lowdown

So, I thought, "Hey, let's make my profile pop with package download stats!".
To figure out the how-to, I checked out the "Analyzing PyPI package downloads" page. The docs there were crystal clear
and super user-friendly. Essentially, all the download stats for PyPi packages hang out in the GCP BigQuery Data
Warehouse's public database. So, I hopped into the GCP BigQuery Studio and started tinkering. 
A swift search and a few lines of SQL later, I had download stats neatly encapsulated in a query. 
Little did I know, this was just the beginning of an engaging journey.

## The "Aha!" Moment

Despite being an AWS enthusiast, there's something irresistibly straightforward about GCP BigQuery. 
Once you're in the console, a quick search for PyPi public data is as easy as typing: 
"bigquery-public-data.pypi.file_downloads.". Then all you need to do is to type your query in using SQL.

My journey kicked off with the guide's provided sample – 
a straightforward SQL query fetching download count stats for the pytest package in the last 30 days:

```sql
#standardSQL
SELECT COUNT(*) AS num_downloads
FROM `bigquery-public-data.pypi.file_downloads`
WHERE file.project = 'pytest'
  -- Only query the last 30 days of history
  AND DATE(timestamp)
    BETWEEN DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY)
    AND CURRENT_DATE()
```

With a quick adjustment to the package name and dates, all the download stats neatly packed into one query:

```sql
SELECT
  file.project AS package_name,
  COUNT(*) AS download_count,
  MAX(timestamp) AS last_download_timestamp
FROM `bigquery-public-data.pypi.file_downloads`
WHERE file.project IN ('{package_name}')
AND timestamp > '{from_date}'
GROUP BY file.project;
```

The provided SQL query examples play a pivotal role in extracting PyPi package download statistics from the GCP BigQuery Data Warehouse. 
The initial query, centered around the pytest package, retrieves download count statistics for the last 30 days. 
A subsequent adjustment tailors the query to target specific packages and date ranges, neatly packaging the download stats into a comprehensive query.

To expose this functionality through an API, I employed a Lambda function. 
Leveraging the google-cloud-bigquery library, the Lambda function connects with BigQuery, 
authenticates using Google credentials (via the google-auth library), and executes the query. 
Without delving too much into implementation details, a Lambda Layer was created with necessary dependencies, 
and the Google Auth JSON was stored as a Lambda secret, streamlining the authentication process.

The results are then processed and formatted, ensuring seamless integration into the Serverless architecture. 
This code snippet forms the backbone of the data retrieval process, 
establishing the groundwork for a dynamic and responsive API.


```python
import os
import json

from google.cloud import bigquery
from google.oauth2.service_account import Credentials


def get_stat(packages, from_date):
    package_names = "','".join([p.strip() for p in packages])
    query_string = ...
        
    secret_value = json.loads(os.environ.get("GCP_JSON"))
    credentials = Credentials.from_service_account_info(secret_value)
    client = bigquery.Client(credentials=credentials)
    
    # Create a client and execute the query
    query_job = client.query(query_string)
    
    # Fetch all results
    results = list(query_job.result())
    
    stat_data = []
    for item in results:
        data = dict(zip(["package_name", "download_count", "last_download_timestamp"], item.values()))                
        stat_data.append(data)

    return stat_data
```

With this setup, I wrapped it into a Lambda function, ensuring a seamless integration into my Serverless architecture. 
The journey continued! 

## Initial Simple Upgrade

Of course, the initial setup wasn't quite cutting it as querying took a bit of time. 
Desiring a snappier API response, I took the next step. 
Introducing EventBridge Scheduler— a powerful tool allowing the creation of recurrent tasks. 
In our case, it served as the orchestrator, invoking a Lambda function at regular intervals. 
This Lambda function was purposefully designed to fetch the latest PyPi package data and diligently store it in a cache. 
Now, with the API effortlessly retrieving the freshest cache, the upgrade seamlessly enhanced the overall performance. 
Cheers to efficiency! 🚀

## Reality Check

A few days later, I took a peek at my GCP Billing – and whoa, 
a whopping $5 daily for a counter! Cue the panic mode. 
I immediately adjusted the EventBridge scheduler, allowing the function to run only once every 24 hours.
Still, it's not a real solution. Time for a deep dive.

## The "Oh No!" Moment

BigQuery has a voracious appetite for data, and that's a big chunk of the costs. 
The amount of data it processes in each request directly influences the bill. 
Every request contains a date range – a crucial piece of the puzzle.

Of course, I used a date range – the creation date of my oldest OpenSource project, to be precise. 
Why? I wanted to grab the overall downloads counter - from the date the project was released. 
The snag? That project kicked off seven years ago. Explains the costs, oops! 
The fix was clear – adjust Lambda to focus on recent hours, not ages.

The solution turned out to be pretty straightforward. 
With every Lambda invocation, I first read the cache, extracted the latest timestamp, 
and then hunted for new downloads since that date-time, updating the cache. 
Now, with every request, I'm only sifting through the last few hours – not a few years. Crisis averted!

## Problem Solved... Almost

New issue – what about the accumulated costs from the past few days? 
In a stroke of luck, a big shoutout to Google billing support became the saving grace! 
I reached out to detail my billing drama, sharing the twists and turns of my cost exploration.
Much to my relief, the team at Google billing support was understanding and responsive. 
They not only acknowledged my concerns but also went the extra mile to process a refund. 
Phew! The swift and supportive response from Google billing support gracefully resolved the financial hiccup, 
turning a potential headache into a comforting sigh of relief.

## The Final Takeaway


BigQuery proves its awesomeness; whether through the console or Python, it's effortlessly user-friendly. 
However, beware of those sneaky costs. With the adventure now concluded, valuable lessons have been gleaned. 
The profile page successfully upgraded – great success! 🚀
