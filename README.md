# Tweet Processing: Graphika Data Engineer Take-Home
Processing Tweets as a nightly job.

## Requirements
- The raw data (`nodes1.txt`, `nodes2.txt`, `terms1.txt`, `terms2.txt`, `tweets.jsonl.gz`)
- Python 3.7 (a lower 3.x version might work but that's what I used)
- I have included a `requirments.txt` but if you want to skip that read below.
- The two non-standard Python packages that I used are `pandas` and `apscheduler`
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
This will generate a subfolder named `tweets_YYYYMMDD` where `YYYYMMDD` is today's date.

Inside the folder will be timestamped `.csv` versions of the nodes, terms, and raw Tweet data. This is done for historical purposes.

The answers will be saved as `matches1_YYYYMMDD.txt` for `terms1.txt` and `nodes1.txt` applied to `tweets.jsonl.gz`. 

Similarly, `matches2_YYYYMMDD.txt` is for `terms2.txt` and `nodes2.txt` applied to `tweets.jsonl.gz`.

## Nightly Job
The default for the nightly job is 11:00 PM local time.

If you want to test this step I would set it for a +1 min of your current local time. For example, if current time is 3:12 PM change to this:  ```sched.add_job(job_function, 'cron', hour='15', minute="13")```. Save and run:

```shell
$ python3 nightly_job.py
```

Note: for proper recovery and logging this job should be stored in a server (as suggested in the documentation: https://apscheduler.readthedocs.io/en/stable/userguide.html#choosing-the-right-scheduler-job-store-s-executor-s-and-trigger-s). Lacking this, we are trusting that the Python script is allowed to run 24/7, waiting to execute at 11pm. The nodes, terms, and tweet data can be refreshed in the meantime w/o loss of replayability.

## Expanding
If new terms and nodes lists were to be added, all that needs to be done is an additional line to the `main()` part of `process_tweets.py`. Specifically, if `nodes3.txt` and `terms3.txt` were added then all one would need to do is add the following line:
```process_tweets(raw,timeStamp,directoryName,3)```
