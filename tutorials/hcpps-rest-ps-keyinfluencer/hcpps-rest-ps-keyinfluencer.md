---
title: Test the "Key Influencer" service
description: Using a REST client, you will test the "Key Influencer" SAP Cloud Platform predictive service
primary_tag: products>sap-predictive-service
tags: [ tutorial>beginner, topic>machine-learning, products>sap-predictive-service, products>sap-cloud-platform ]
---

## Prerequisites
  - **Proficiency:** Beginner
  - **Tutorials:** [Configure the SAP Predictive services](https://www.sap.com/developer/groups/ps-configure.html)

## Next Steps
- [Test the SAP Predictive services using a REST client](https://www.sap.com/developer/groups/ps-test-rest.html)

## Details
### You will learn
 - How to use the "Key Influencer" SAP Predictive services from a REST Client.

 Only the synchronous mode will be tested here but you can mimic what was done in the [Test the "Forecast" SAP Predictive services using a REST client](https://www.sap.com/developer/tutorials/hcpps-rest-ps-forecast.html) tutorial for the asynchronous mode.

### Time to Complete
  **10 minutes**

> In order to ease the readability of this tutorial, we have used tokens to replace long URLs.
> Therefore you can replace any occurrence of the token by the value listed above:
>
> - <code><b>C4PAURL</b></code> : represents the predictive services **Application URL** displayed on the overview page and should look like this (XYZ is your SAP Cloud Platform account name):
>     
```
     https://aac4paservicesXYZ.hanatrial.ondemand.com/com.sap.aa.c4pa.services
```
>      
>
> If you are unclear with what is your SAP Cloud Platform account name, you can refer to the following blog entry: [SAP Cloud Platform login, user name, account id, name or display name: you are lost? Not anymore!](https://blogs.sap.com/2017/01/31/sap-hana-cloud-platform-trial-login-name-user-name-account-name-account-identifier-you-are-lost-not-anymore/)

&nbsp;

> **Note**: if you are running into some issue, you can check the [SAP Predictive services Troubleshooting guide](https://www.sap.com/developer/tutorials/hcpps-troubleshoot.html) to diagnose the most common ones.

[ACCORDION-BEGIN [Info:](A short description of the Key Influencer service)]
The Key Influencers service analyzes a dataset to identify the variables with an influence on a specified target variable.

This service:

 - Identifies the variables with an influence on a specified target ordered by decreasing contribution
 - Returns detailed information on the grouped categories for each contributive variable
 - Provides indicators on the reliability of the results

> **Note**: The target variable must be either binary nominal or continuous. Multinomial targets are not supported.

&nbsp;

To summarize, in order to execute the key influencer service, you will need a dataset with:

 - a target variable for which you want to find the influencers variables
 - a set of variables that potentially influence the target variable.

Optionally, you can define the following parameters to enhance your analysis:

 - auto-selection: flag that indicates if you want to shorten the number of retained influencers
 - number of influencers: the number of key influencers to return
 - skipped variables: a list of variables to skip from the analysis
 - target value: in the case of binary target variable, it indicates the target variable value of interest. By default the value with the lowest frequency will be picked.
 - variable description: a more details description of the dataset
 - weight variable: a column to be used to increase the importance of a row

[ACCORDION-END]

[ACCORDION-BEGIN [Info:](A short description of the Census dataset)]

The dataset will be using during this tutorial is extracted from the sample dataset available with SAP Predictive Analytics.

The Census sample data file that you will use to follow the scenarios for Regression/Classification and Segmentation/Clustering is an excerpt from the American Census Bureau database, completed in 1994 by Barry Becker.

> **Note:** For more information about the American Census Bureau, see <http://www.census.gov> published on non-SAP site.

&nbsp;

This file presents the data on 48,842 individual Americans, of at least 17 years of age. Each individual is characterized by 15 data items. These data, or variables, are described in the following table.

Variable | Description | Example of Values
:------------- | :-------------- | :--------------
<nobr>`age`</nobr> | Age of individuals | Any numerical value greater than 17
<nobr>`workclass`</nobr> | Employer category of individuals | Private, Self-employed-not-inc, ...
<nobr>`fnlwgt`</nobr> | Weight variable, allowing each individual to represent a certain percentage of the population | Any numerical value, such as 0, 2341 or 205019
<nobr>`education`</nobr> | Level of study, represented by a schooling level, or by the title of the degree earned | 11th, Bachelors
<nobr>`education_num`</nobr> | Number of years of study, represented by a numerical value | A numerical value between 1 and 16
<nobr>`marital_status`</nobr> | Marital status | Divorced, Never-married, ...
<nobr>`occupation`</nobr> | Job classification | Sales, Handlers-cleaners, ...
<nobr>`relationship`</nobr> | Position in family | Husband, Wife, ...
<nobr>`race`</nobr> | Ethnicity |
<nobr>`sex`</nobr> | Gender | Male, Female, ...
<nobr>`capital_gain`</nobr> | Annual capital gains | Any numerical value
<nobr>`capital_loss`</nobr> | Annual capital losses | Any numerical value
<nobr>`native country`</nobr> | Country of origin| United States, France, ...
<nobr>`class`</nobr> | Variable indicating whether or not the salary of the individual is greater or less than $50,000| "1" if the individual has a salary of greater than $50,000 & "0" if the individual has a salary of less than $50,000

[ACCORDION-END]  

[ACCORDION-BEGIN [Step 1: ](Register the Census dataset)]

First we need to register the Census dataset.

As described in [**Step 1** of **Test the "Dataset" services** tutorial](https://www.sap.com/developer/tutorials/hcpps-rest-ps-dataset.html), register the Census dataset using the following elements:

Open a new tab in ***Postman***.

> If you don't have ***Postman*** installed yet, you can refer to the following how-to guide: [Install Postman extension for Google Chrome as a REST client](https://www.sap.com/developer/tutorials/api-tools-postman-install.html)

&nbsp;

Field Name     | Value
:------------- | :--------------
Request Type   | <code><b>POST</b></code>
URL            | <code><b>C4PAURL</b></code>`/api/analytics/dataset/sync`

```json
{
  "location": {
    "schema" : "PSDEMO",
    "table" : "Census"
  }
}
```

**Take note of the returned dataset identifier.**

[ACCORDION-END]    

[ACCORDION-BEGIN [Step 2: ](Run the Key Influencer service)]

Open a new tab in ***Postman***.

Fill in the following information:

Field Name     | Value
:------------- | :--------------
Request Type   | <code><b>POST</b></code>
URL            | <code><b>C4PAURL</b></code>`/api/analytics/keyinfluencer/sync`

![Postman URL](01.png)

Select the **Authorization** tab and fill in the following information:

Field Name     | Value
:------------- | :-------------
Type           | `Basic Auth`
Username       | your ***SAP Cloud Platform Account*** login (usually the email address used to register your ***SAP Cloud Platform*** account)
Password*      | your ***SAP Cloud Platform Account*** password

![Postman URL](02.png)

Select the **Body** tab, enable the **raw** mode and select `JSON (application/json)` in the drop down, then add the following content:

```json
{
  "datasetID": 9999999,
  "targetColumn": "class",
  "numberOfInfluencers" : 2,
  "targetKey" : 1,
  "skippedVariables" : ["id", "sex", "race"]
}
```
> Make sure the `datasetID` (here the value 9999999) is correct. To get the list of valid identifier, you can run ***Step 6: List all registered datasets*** from the [Test the "Data Set" SAP Predictive services using a REST client](https://www.sap.com/developer/tutorials/hcpps-rest-ps-dataset.html) tutorial.


&nbsp;

With these settings, we will get the top 2 influencers of the class variable, excluding the "id", "sex", "race" variables from the analysis.

Click on **Send**

Congratulations! You have just run a key influencer service on the Census dataset.

Here is the result:

```
{
  "excludedVariables": [],
  "influencers": [
    {
      "contribution": 0.23821930990966,
      "groups": [
        {
          "frequency": 0.18009400511255874,
          "group": "{Divorced;Married-AF-spouse;Married-spouse-absent;Widowed}",
          "groupDefinition": {
            "categories": [
              "Divorced",
              "Married-AF-spouse",
              "Married-spouse-absent",
              "Widowed"
            ],
            "higherBound": null,
            "higherBoundIncluded": null,
            "kxmissingIncluded": 0,
            "lowerBound": null,
            "lowerBoundIncluded": null
          },
          "normalProfit": -0.13836921830448676,
          "significance": -0.13727210597294148,
          "targetMean": 0.09996947496947498
        },
        {
          "frequency": 0.4584261015365163,
          "group": "{Married-civ-spouse}",
          "groupDefinition": {
            "categories": [
              "Married-civ-spouse"
            ],
            "higherBound": null,
            "higherBoundIncluded": null,
            "kxmissingIncluded": 0,
            "lowerBound": null,
            "lowerBoundIncluded": null
          },
          "normalProfit": 0.2055394695753008,
          "significance": 0.5190487162104204,
          "targetMean": 0.4438781628492625
        },
        {
          "frequency": 0.36147989335092495,
          "group": "{Never-married;Separated}",
          "groupDefinition": {
            "categories": [
              "Never-married",
              "Separated"
            ],
            "higherBound": null,
            "higherBoundIncluded": null,
            "kxmissingIncluded": 0,
            "lowerBound": null,
            "lowerBoundIncluded": null
          },
          "normalProfit": -0.19172626836330856,
          "significance": -0.38177661023747916,
          "targetMean": 0.046612424910653184
        }
      ],
      "variable": "marital_status"
    },
    {
      "contribution": 0.19219598255964018,
      "groups": [
        {
          "frequency": 0.005964651878727908,
          "group": "{3103;4386;4787}",
          "groupDefinition": {
            "categories": [
              "3103",
              "4386",
              "4787"
            ],
            "higherBound": null,
            "higherBoundIncluded": null,
            "kxmissingIncluded": 0,
            "lowerBound": null,
            "lowerBoundIncluded": null
          },
          "normalProfit": 0.6694954081085267,
          "significance": 0.021997648428929345,
          "targetMean": 0.9078341013824884
        },
        {
          "frequency": 0.04224732690140458,
          "group": "{5178;7298;7688;8614;9386;10520;13550;14084;14344;15024;20051;27828;99999}",
          "groupDefinition": {
            "categories": [
              "10520",
              "13550",
              "14084",
              "14344",
              "15024",
              "20051",
              "27828",
              "5178",
              "7298",
              "7688",
              "8614",
              "9386",
              "99999"
            ],
            "higherBound": null,
            "higherBoundIncluded": null,
            "kxmissingIncluded": 0,
            "lowerBound": null,
            "lowerBoundIncluded": null
          },
          "normalProfit": 0.7616613067260383,
          "significance": 0.17725752508361203,
          "targetMean": 1
        },
        {
          "frequency": 0.01720678376075424,
          "group": "{594;1055;2174;2176;2597;2829;2885;3137;3325;3411;3464;3908;4064;4101;4650;4865;5013;6849}",
          "groupDefinition": {
            "categories": [
              "1055",
              "2174",
              "2176",
              "2597",
              "2829",
              "2885",
              "3137",
              "3325",
              "3411",
              "3464",
              "3908",
              "4064",
              "4101",
              "4650",
              "4865",
              "5013",
              "594",
              "6849"
            ],
            "higherBound": null,
            "higherBoundIncluded": null,
            "kxmissingIncluded": 0,
            "lowerBound": null,
            "lowerBoundIncluded": null
          },
          "normalProfit": -0.23833869327396173,
          "significance": -0.02259112233850596,
          "targetMean": 0
        },
        {
          "frequency": 0.9345812374591133,
          "group": "{KxOther;0}",
          "groupDefinition": {
            "categories": [
              "0",
              "KxOther"
            ],
            "higherBound": null,
            "higherBoundIncluded": null,
            "kxmissingIncluded": 0,
            "lowerBound": null,
            "lowerBoundIncluded": null
          },
          "normalProfit": -0.034315282197816904,
          "significance": -0.17666405117403566,
          "targetMean": 0.20402341107614483
        }
      ],
      "variable": "capital_gain"
    }
  ],
  "modelPerformance": {
    "confidenceIndicator": 1,
    "predictionConfidence": 0.9899,
    "predictivePower": 0.8358,
    "qualityRating": 5
  },
  "parameters": {
    "autoSelection": true,
    "datasetID": 13,
    "numberOfInfluencers": 2,
    "skippedVariables": [
      "id",
      "sex",
      "race"
    ],
    "targetColumn": "class",
    "targetKey": 1
  }
}
```

The top 2 influencers are the `marital_status` and `capital_gain`. They have been picked because they have the highest contribution in the model. The model also provide you the groups resulting from the ***encoding*** phase.

But if you look closer you will notice that the `capital_gain` has been considered as nominal instead of continuous.

Let's fix that with a more accurate variable description.

[ACCORDION-END]    

[ACCORDION-BEGIN [Step 3: ](Adjust the variable description)]

Just like in the Forecast service, we could adjust the variable description at the dataset level, but we will instead use the Key Influencers service parameters to adjust the variable description.

Open a new tab in ***Postman***.

Fill in the following information

Field Name     | Value
:------------- | :--------------
Request Type   | <code><b>POST</b></code>
URL            | <code><b>C4PAURL</b></code>`/api/analytics/keyinfluencer/sync`

Select the **Authorization** tab and fill in the same details as in the previous call.

Select the **Body** tab, enable the **raw** mode and select `JSON (application/json)` in the drop down, then add the following content:

```json
{
  "datasetID": 9999999,
  "targetColumn": "class",
  "numberOfInfluencers" : 2,
  "targetKey" : 1,
  "skippedVariables" : ["id", "sex", "race"],
  "variableDescription" : [
    {"position" : "1", "variable" : "id", "storage" : "integer" , "value" : "nominal" ,  "key" : "1"},
  	{"position" : "2", "variable" : "age", "storage" : "integer" , "value" : "continuous"},
  	{"position" : "3", "variable" : "workclass", "storage" : "string" , "value" : "nominal"},
  	{"position" : "4", "variable" : "fnlwgt", "storage" : "integer" , "value" : "continuous"},
  	{"position" : "5", "variable" : "education", "storage" : "string" , "value" : "nominal"},
  	{"position" : "6", "variable" : "education_num", "storage" : "integer" , "value" : "ordinal"},
  	{"position" : "7", "variable" : "marital_status", "storage" : "string" , "value" : "nominal"},
  	{"position" : "8", "variable" : "occupation", "storage" : "string" , "value" : "nominal"},
  	{"position" : "9", "variable" : "relationship", "storage" : "string" , "value" : "nominal"},
  	{"position" : "10", "variable" : "race", "storage" : "string" , "value" : "nominal"},
  	{"position" : "11", "variable" : "sex", "storage" : "string" , "value" : "nominal"},
  	{"position" : "12", "variable" : "capital_gain", "storage" : "integer" , "value" : "continuous"},
  	{"position" : "13", "variable" : "capital_loss", "storage" : "integer" , "value" : "continuous"},
  	{"position" : "14", "variable" : "hours_per_week", "storage" : "number" , "value" : "continuous"},
  	{"position" : "15", "variable" : "native_country", "storage" : "string" , "value" : "nominal"},
  	{"position" : "16", "variable" : "class", "storage" : "integer" , "value" : "nominal"}
  ]
}
```
> Make sure the `datasetID` (here the value 9999999) is correct. To get the list of valid identifier, you can run ***Step 6: List all registered datasets*** from the [Test the "Data Set" SAP Predictive services using a REST client](https://www.sap.com/developer/tutorials/hcpps-rest-ps-dataset.html) tutorial
&nbsp;

Click on **Send**

The top 2 influencers are still the `marital_status` and `capital_gain`. But if you look closer you will notice that the `capital_gain` is now properly encoded as a continuous attribute. However, you can see that one of the group for the `capital_gain` variable has a `higherBound` equal to "99999" which is in fact the missing value indicator.

Let's try with the following **Body**, which define the missing value for the relevant attribute:
```json
{
  "datasetID": 9999999,
  "targetColumn": "class",
  "numberOfInfluencers" : 2,
  "targetKey" : 1,
  "skippedVariables" : ["id", "sex", "race"],
  "variableDescription" : [
    {"position" : "1", "variable" : "id", "storage" : "integer" , "value" : "nominal" ,  "key" : "1"},
  	{"position" : "2", "variable" : "age", "storage" : "integer" , "value" : "continuous"},
  	{"position" : "3", "variable" : "workclass", "storage" : "string" , "value" : "nominal" ,  "missing" : "?"},
  	{"position" : "4", "variable" : "fnlwgt", "storage" : "integer" , "value" : "continuous"},
  	{"position" : "5", "variable" : "education", "storage" : "string" , "value" : "nominal"},
  	{"position" : "6", "variable" : "education_num", "storage" : "integer" , "value" : "ordinal"},
  	{"position" : "7", "variable" : "marital_status", "storage" : "string" , "value" : "nominal"},
  	{"position" : "8", "variable" : "occupation", "storage" : "string" , "value" : "nominal" ,  "missing" : "?"},
  	{"position" : "9", "variable" : "relationship", "storage" : "string" , "value" : "nominal"},
  	{"position" : "10", "variable" : "race", "storage" : "string" , "value" : "nominal"},
  	{"position" : "11", "variable" : "sex", "storage" : "string" , "value" : "nominal"},
  	{"position" : "12", "variable" : "capital_gain", "storage" : "integer" , "value" : "continuous" ,  "missing" : "99999"},
  	{"position" : "13", "variable" : "capital_loss", "storage" : "integer" , "value" : "continuous"},
  	{"position" : "14", "variable" : "hours_per_week", "storage" : "number" , "value" : "continuous"},
  	{"position" : "15", "variable" : "native_country", "storage" : "string" , "value" : "nominal" ,  "missing" : "?"},
  	{"position" : "16", "variable" : "class", "storage" : "integer" , "value" : "nominal"}
  ]  
}
```

> Make sure the `datasetID` (here the value 9999999) is correct. To get the list of valid identifier, you can run ***Step 6: List all registered datasets*** from the [Test the "Data Set" SAP Predictive services using a REST client](https://www.sap.com/developer/tutorials/hcpps-rest-ps-dataset.html) tutorial
&nbsp;

The performances a roughly the same as we limited the analysis to only 2 attributes on a small dataset. Off course you can increase the number of influencers and see the difference.

[ACCORDION-END]

### Optional

For more details on the SAP Predictive services, you can check the following the [`Key Influencers APIs`](https://help.sap.com/viewer/20cd1b0396db4826a9b76b4ce869f00a/Cloud/en-US/e8bda9b653ff4b48a702124e27747f85.html) documentation.

## Next Steps
- [Test the SAP Predictive services using a REST client](https://www.sap.com/developer/groups/ps-test-rest.html)
