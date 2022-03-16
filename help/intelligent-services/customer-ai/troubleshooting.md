---
keywords: Experience Platform;getting started;customer ai;popular topics;customer ai input;customer ai output;customer ai troubleshooting;customer ai errors
solution: Experience Platform, Intelligent Services, Real-time Customer Data Platform
feature: Customer AI
title: Customer AI error troubleshooting
topic-legacy: Getting started
description: Find answers to common errors in Customer AI.
type: Documentation
exl-id: 37ff4e85-da92-41ca-afd4-b7f3555ebd43
source-git-commit: 896dda631cd4182f278de0607bea442d8366fe8c
workflow-type: tm+mt
source-wordcount: '529'
ht-degree: 0%

---

# Customer AI error troubleshooting

Customer AI displays errors when model training, scoring, and configuration fails. ********************

![](./images/errors/last-run-status.png)

******************** In the event that Customer AI is not able to provide details on your error, contact support with the error code thats provided.

<img src="./images/errors/last-run-details.png" width="300" /><br />

## Unable to access Customer AI in Chrome incognito

Loading errors in Google Chrome&#39;s incognito mode are present because of updates in Google Chrome’s incognito mode security settings. The issue is actively being worked on with Chrome to make experience.adobe.com a trusted domain.

<img src="./images/errors/error.PNG" width="500" /><br />

### Recommended fix

To workaround this issue you need to add experience.adobe.com as a site that can always use cookies. ************`[*.]experience.adobe.com`********

![](./images/errors/cookies2.gif)

## Model quality is poor

Follow the recommended steps below to help troubleshoot.

<img src="./images/errors/model-quality.png" width="300" /><br />

### Recommended fix

&quot;Model quality is poor&quot; means that the model accuracy is not within an acceptable range. Customer AI was unable to build a reliable model and AUC (Area under the ROC curve) &lt; 0.65 after training. To fix the error, it is recommended that you change one of the configuration parameters and rerun the training.

Start by checking the accuracy of your data. It is important that your data contains the necessary fields needed for your predictive outcome.

- Check whether your dataset has the latest dates. Customer AI always assumes that the data is up-to-date when the model is triggered.
- Check for missing data within your defined prediction and eligibility window. Your data needs to be complete with no gaps. [](./input-output.md#data-requirements)
- Check for missing data in commerce, application, web, and search, within your schema field properties.

`_experience.analytics.customDimensions.eVars.eVar142`This restricts the population and size of the data used in the training window.

If restricting the eligibility population did not work or is not possible, change your prediction window.

- Try changing your prediction window to 7 days and see if the error continues to occur. If the error no longer occurs, this indicates that you may not have enough data for your defined prediction window.
