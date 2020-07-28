---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform でのデータ保護
topic: data protection
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 18%

---


# Adobe Experience Platform でのデータ保護

All data that is ingested and used by Adobe Experience Platform is stored in the [!DNL Data Lake], a highly granular data store containing all data managed by [!DNL Platform], regardless of origin or file format. All data persisted in the [!DNL Data Lake] is encrypted, stored, and managed in an isolated [!DNL Microsoft Azure Data Lake] Storage account that is unique to your organization.

The following process flow diagram illustrates how data is ingested, processed, encrypted, and persisted by [!DNL Experience Platform]:

![](images/data-protection/flow.png)

For details on how data at rest is encrypted in [!DNL Data Lake Storage], see the document on [data encryption in Azure Data Lake Storage](https://docs.microsoft.com/JA-JP/azure/data-lake-store/data-lake-store-encryption). For information on how data at rest is encrypted in [!DNL Cosmos DB], see the document on [data encryption in Azure Cosmos DB](https://docs.microsoft.com/JA-JP/azure/cosmos-db/database-encryption-at-rest).