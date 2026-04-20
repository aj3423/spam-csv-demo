### What is this
This repo demonstrates syncing spam numbers from [SpamBlocker](https://github.com/aj3423/SpamBlocker/) to your github repo.

### What it does
- Reporting a number in SpamBlocker (manually or automatically) triggers the github REST API
- Which triggers the github action workflow
- The workflow then adds the number to the **spam.csv**

### Set up
1. Create a github action workflow like this: [add_spam_number.yml](https://github.com/aj3423/spam-csv-demo/blob/master/.github/workflows/add_spam_number.yml)
2. Create a github token (see https://github.com/settings/tokens)
3. In SpamBlocker, import this as a "Report Number" workflow

   (replace the github repo/token with yours)
   ```json
   {
    "type": "ReportApi",
    "id": 1,
    "desc": "spam CSV demo",
    "actions": [
        {
            "type": "ParseIncomingNumber",
            "numberFilter": ".*",
            "forwardType": 0
        },
        {
            "type": "HttpDownload",
            "method": 1,
            "url": "https://api.github.com/repos/aj3423/spam-csv-demo/actions/workflows/add_spam_number.yml/dispatches",
            "header": "{bearer_auth(ghp________your_github_token___________)}",
            "body": "{\"ref\":\"master\", \"inputs\":{\"phone_number\":\"{origin_number}\"}}",
            "enableRetry": false,
            "retryTimes": 0,
            "retryDelayMs": 0
        }
    ],
    "enabled": true,
    "autoReportTypes": 7
   }
   ```
 
