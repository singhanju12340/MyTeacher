---
Creation Time: Monday, January 20th 2025
Modified Time: Monday, January 20th 2025
---

Write a cron method which will read the payment information from a db(DB can be anything) and trigger a process to complete the payment

No need to create the cron job or schedule. Ask him/her to write a method that will be triggered by cron. This method should read the data from db and process the payment and update the status

what to check:  

- how error conditions are handled
- how to handle retry mechanism
- how to handle concurrency (like 1 cron is running and other cron should not pickup the same record to process)

name: anju
payment pending: 30K
assume payment gatway exist


payment class 
process payment
handle payment failure: insufficient balance/ gateway failure , retry for the same after certain duration
handle concurrency
idempotent request
if 1 cron is running for 1 payment
other scheduled cron should not process the same payment
notification


Read details from DB
process payment from read db
process payment

SSE:
Erro handling
retry 
flag
