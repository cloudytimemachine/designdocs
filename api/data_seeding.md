# Executive Summary

In an ideal world, Cloudy Time Machine would have lots of users constantly requesting snapshots. Until this project is smashingly successful, we need a mechanism to seed our database with snapshots of popular websites over a period of time.

We should make requests using the new REST API to archive the top X websites at least once a day. As a starting point, see [this list of top 500 websites](https://moz.com/top500/domains/csv).

# Goals
* Archive the top X websites at least once a day
* Leverage public facing REST api
* Should run inside the existing CTM cluster (sandbox)

# Non-goals
* CI/CD for this application
* Automated tests

# Background
N/A

# High-Level Design
The data_seeder will be a lightweight application that hits the REST API to create new snapshots at given intervals. The list of data to request and the schedule of requests shall be configurable via config file or command line.

# Detailed Design
data_seeder is a node.js app running inside a temporary container in our cluster. A very basic starting point is the test branch of CTM/api that simply fires off a bunch of POST requests to the API with a wrapper around it to make it run regularly and with smarter logic.

The app will read in a list of sites to archive at runtime from a config file. Node-cron will handle the scheduling.

Major design consideration is throttling/scheduling the requests so the usability of the snapshot queue is never severely impacted by the data_seeder app. In this case, we'll throttle the requests by submitting them in small bunches at regular intervals so by the end of the day we have the full lot.

# Alternatives Considered
* Scheduling:
  - Kubernetes 1.4 Scheduled jobs are not yet an option. We're on 1.3.
  - Cron in a container is a possibility, but I think node-cron more explicitly internalizes the scheduling to the app.
* Throttling:
  - data_seeder could check the queue length before submitting bot jobs and if the queue has more than some threshold of jobs in it, retry the submission a little later. This solution is fine but I think submitting spaced out bunches of requests is likely simpler. If we run into issues with an overloaded queue, this is the next approach.
  - We could do a dual priority queue. High priority for human search and bots at another lower tier. This may be something to think about in the future, but it means changes all over the rest of code-base.

# Security Concerns
N/A

# Open Issues
* Do we want to log the activities of data_seeder somewhere?
* Are we doing any authentication on the REST API/Plans for it?
