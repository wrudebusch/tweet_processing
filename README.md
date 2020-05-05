# Tweet Processing: Graphika Data Engineer Take-Home 
Processing Tweets as a nightly job. 

## Requirments
- The raw data (`nodes1.txt`, `nodes2.txt`, `terms1.txt`, `terms2.txt`, `tweets.jsonl.gz`)
- Python 3.7 (a lower 3.x version might work but that's what I used)
- I have included a `requirments.txt` but if you want to skip that read below.
- The two non-standard Python pacakges that I used are `pandas` and `apscheduler` 
- Here is one way to install them:
```shell
$ python3 -m pip install --user pandas
$ python3 -m pip install --user apscheduler
```

## Processing Tweets
If run by itself 
```shell
$ python3 process_tweets.py 
```
This will generate a folder named `tweets_YYYYMMDD` where `YYYYMMDD` is today's date. 
Inside the folder will be timestamped `.csv` versions of the nodes, terms, and raw Tweet data. this is for historical prupsoes. 

The asnwers will be saved as `matches1_YYYYMMDD.txt` for `terms1.txt` and `nodes1.txt` applied to `tweets.jsonl.gz`. Similarly, `matches2_YYYYMMDD.txt` is for `terms2.txt` and `nodes2.txt` applied to `tweets.jsonl.gz`. 

## Nightly Job
The default for the nightly job is 11:00 PM local time. 

```shell
$ python3 nightly_job.py 
```

If you want to test to see if this works I would set it for a 1 min future of your current time. 

Note: in production this job shoudl be stored in a server. Lacking this, we are trusting that the Python script is allowed to run 24/7, waiting to execute at 11pm.
