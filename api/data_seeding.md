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
*(terminology and history one needs to know to understand the design)*

# High-Level Design
*(a brief, high-level description of the design; diagrams are helpful)*

# Detailed Design
*(the design in detail; usually a top-down description)*

# Alternatives Considered
*(alternatives considered and rejected)*

# Security Concerns
*(security/privacy ramifications and how those concerns are mitigated)*

# Open Issues
*(questions regarding requirements, mockups, etc)*
