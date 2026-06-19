---
up: "[[Project Shaman]]"
tags:
  - type/product-feature
created-date: 2026-03-15
related:
summary: How to store and process trace-data
---

Process the trace data in 100 data-points

Traces data should be aggregated by function. When every the trace data received from gateway for processing the hit-address should be resolved to function. When the trace detail page is opened the table view will show all the functions which are coverage and will have stats like total number of basic block in identified in the coverage-map vs block which are coverage and table will have on more column which show the percentage coverage.


I want you to test the start tracing feature via attach process I have already connected a device 'jbhbpd', i want you to run the pipeline process on 'chrome'. Get the list of all the processing running a and pick just one 'chrome' process and execute rest of the pipeline, do this via api call and watch out for error in the 'logs' logs. This call should trigger celery task so monitor that via logs or redis whatever seems feasible to you, also monitor the tracing progress via API call. All the stages should execute properly. if not start filing bugs via 'beads'