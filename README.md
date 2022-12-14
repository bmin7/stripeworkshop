# Stripe DaC Workshop
Welcome to the Detections Workshop. This guide will provide you with a step-by-step of all the commands we will use throughout this workshop. Please reference it as we move forward. If you have questions, feel free to ask your group moderator.




## Exercise 1 - Using Panther Packs to write a new detection
We will use a pre-packaged detection from Panther and modify it to our liking within the Panther Console.

**Terms we'll reference**
- [Functions Template](https://github.com/panther-labs/panther-analysis/blob/master/templates/example_rule.py)
- [Rules](https://docs.panther.com/writing-detections/rules)
- [Packs](https://docs.panther.com/writing-detections/detection-packs)
- [Helpers](https://docs.panther.com/writing-detections/globals?q=helpers)
- [Deep_Get](https://docs.panther.com/writing-detections/globals#deep_get)

**Exercise 1 Steps**
1. In the Panther Console - Navigate to Build > Packs > Okta Pack
2. Select the Okta.APIKey.Created rule 
3. In a new tab, create a new rule
4. Name the detection a unique name 
5. Copy and Paste the code from Okta.APIKey.Created 
6. Modify the code
7. Copy over the test event with the sample data from Okta Sample Data Below
8. Run a Unit Test 
9. Add the severity function from the rule functions template in line 11
10. Save Changes


**Sample Data for Okta**
```
{
	"debugContext": {},
	"published": "2021-01-08 21:28:34.875",
	"eventType": "system.api_token.create",
	"version": "0",
	"legacyEventType": "api.token.create",
	"outcome": {
		"result": "SUCCESS"
	},
	"request": {},
	"uuid": "2a992f80-d1ad-4f62-900e-8c68bb72a21b",
	"severity": "INFO",
	"displayMessage": "Create API token",
	"actor": {
		"alternateId": "user@example.com",
		"displayName": "Test User",
		"id": "00u3q14ei6KUOm4Xi2p4",
		"type": "User"
	},
	"target": [
		{
			"id": "00Tpki36zlWjhjQ1u2p4",
			"type": "Token",
			"alternateId": "unknown",
			"displayName": "test_key",
			"details": null
		}
	]
}
```

## Exercise 2 - Enrich a Detection with GreyNoise
We're going to walk through how to use an enrichment provider in product to enrich a detection and alert. 

- [GreyNoise](https://docs.panther.com/enrichment/greynoise)
- [Lookup Tables](https://docs.panther.com/enrichment/lookup-tables)

** Sample Data for Brute Force Detection
```
{
	"actor": {
		"alternateId": "admin",
		"displayName": "unknown",
		"id": "unknown",
		"type": "User"
	},
	"client": {
		"ipAddress": "111.111.111.111"
	},
	"eventType": "user.session.start",
	"outcome": {
		"reason": "VERIFICATION_ERROR",
		"result": "FAILURE"
	},
	"p_enrichment": {
		"greynoise_noise_basic": {
			"client.ipAddress": {
				"actor": "EviLCorp",
				"classification": "benign",
				"ip": "1.2.3.4"
			}
		}
	},
	"p_event_time": "2021-06-04 09:59:53.650807",
	"p_log_type": "Okta.SystemLog",
	"p_parse_time": "2021-06-04 10:02:33.650807"
}
```








## Exercise 3 - Test your knowledge
Write a detection for each of the following scenarios and run a passing unit test. Once you've completed all three - submit your results to the Panther Console. First to finish all three in the fastest time, wins!

**Steps for Each Rule**
1. Create a new rule in the Panther Console
2. Give it a unique name with your initials
3. Under the Functions & Test Tab - scroll to the bottom and create a new unit test
4. Copy and paste the example data for the rule you're working on below
5. Write a Python Rule 
6. Test and Verify

**Resources that will help**
- [Documentation](https://docs.panther.com/)
- [Common Helper Functions](https://docs.panther.com/writing-detections/globals#common-helpers)


**Rule 1 - Github New Repository Created**
```
{
	"repo": "my-org/my-repo",
	"actor": "cat",
	"action": "repo.create",
	"created_at": 1621305118553,
	"org": "my-org",
	"p_log_type": "GitHub.Audit"
}
```

- Prompt 1 - Write a detection that fires an alert when a new Github Repository is created by a user 
- Prompt 2 - Create a description and runbook and add it into the alert





**Rule 2 - Box Untrusted Device Access**
```
{
	"created_by": {
		"id": "12345678",
		"type": "user",
		"login": "cat@example",
		"name": "Bob Cat"
	},
	"event_type": "DEVICE_TRUST_CHECK_FAILED",
	"source": {
		"login": "lukeskywalker@starwars.com",
		"id": "12345678",
		"type": "user"
	},
	"type": "event",
	"additional_details": "{\"key\": \"value\"}"
}
```



- Prompt 1 - Write a detection that fires an alert when a user device trust check does not pass
- Prompt 2 - Fire an alert with Info severity when the login user is lukeskywalker@starwars.com and has a Critical severity alert when the user is darthvadar@starwars.com. For all other alerts, severity should be Low



**Rule 3 - AWS CloudTrail - Root Password Change**
```
{
	"recipientAccountId": "123456789012",
	"requestParameters": null,
	"responseElements": {
		"PasswordUpdated": "Success"
	},
	"sourceIPAddress": "111.111.111.111",
	"eventName": "PasswordUpdated",
	"eventSource": "signin.amazonaws.com",
	"eventVersion": "1.05",
	"userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.120 Safari/537.36",
	"eventID": "1111",
	"eventType": "AwsConsoleSignIn",
	"requestID": "1111",
	"userIdentity": {
		"principalId": "123456789012",
		"type": "Root",
		"accesKeyId": "1111",
		"accessKeyId": "",
		"accountId": "123456789012",
		"arn": "arn:aws:iam::123456789012:root"
	},
	"awsRegion": "us-east-1",
	"eventTime": "2019-01-01T00:00:00Z"
}
```


- Prompt 1 - Write a detection that fires off an alert when there is an updated password on a root account 
- Prompt 2 - Add the AWS account ID into the Alert Title 




## Bonus Demo- Locally write a detection and upload it into Panther
Utilizing the Panther Analysis Tool to create, test, and upload a new detection on your local machine. 


**Terms we'll reference**
- [Panther Analysis Tool](https://docs.panther.com/panther-developer-workflows/panther-analysis-tool#overview)
- [API Key](https://docs.panther.com/panther-developer-workflows/api#how-to-use-panthers-api)


**Exercise 2 Steps**
1. Install Panther Analysis Tool 
```pip install panther_analysis_tool```
2. Verify proper version 
```panther_analysis_tool --version```
3. Fork off Panther Analysis Tool to local 
```git clone https://github.com/panther-labs/panther-analysis.git```
4. Create API Token in Panther Console 
5. Check permissions for Read Panther Settings Info, Bulk Upload, Manage Policies, Manage Rules, Manage Schedule Queries, View Log Sources, Manage Log Sources
6. Use Check-Connection to verify API setup is successful
```panther_analysis_tool check-connection --api-host DOMAIN --api-token TOKEN```
6. Create new directory and copy a .py and .yml file
7. Modify .py file and .yml file
8. Test the rule
```panther_analysis_tool test --path <path to rule directory>```
9. Once verified, upload the rule
```panther_analysis_tool upload --path <path to rule> --api-host DOMAIN --api-token TOKEN```
10. Check Panther Console for changes
