---
Creation Time: Thursday, September 12th 2024
Modified Time: Thursday, September 12th 2024
---
## Response delay and timeout due to alert response delay
internal alert lib was blocking thread
some api calls where taking too much time
new relic  we analysed no of timeouts when client apis was failing
then debugged via logs
alert was not made async
alert calls web-hook url, webhook was slow due to heigh load

