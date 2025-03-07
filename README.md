**Sentinel KQL Query to find out the Users Login Hours**

**This query is looking for login activity for a specific user (GGGG) on devices over the past 30 days. It then calculates the first and last login times for each day, the total time the user was active on each day (in hours), and returns a result showing the date, the first and last login times, and the total working duration for that day.**

union Device*
| where TimeGenerated >= ago(30d)
| where AccountName contains "GGGG"  // Fixed this line for string filtering
| summarize FirstEventTime=min(TimeGenerated), LatestEventTime=max(TimeGenerated) by bin(TimeGenerated, 1d)
| extend WorkingDurationHours = datetime_diff('minute', LatestEventTime, FirstEventTime) / 60.0  // Corrected this line for duration calculation
| project Date=TimeGenerated, FirstEventTime, LatestEventTime, WorkingDurationHours
| order by Date asc
