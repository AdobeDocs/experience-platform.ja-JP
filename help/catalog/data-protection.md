---
keywords: Experience Platform;home;popular topics;catalog;data protection;encryption data lake
solution: Experience Platform
title: Adobe Experience Platform でのデータ保護
topic: data protection
description: データレイクに保持されるすべてのデータは、組織に固有の独立した Azure Data Lake Storage アカウントで暗号化、保存、管理されます。次のプロセスフロー図は、データがExperience Platformによって取り込まれ、処理、暗号化され、持続される方法を示しています。
translation-type: tm+mt
source-git-commit: 14f99c23cd82894fee5eb5c4093b3c50b95c52e8
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 28%

---


# Adobe Experience Platform でのデータ保護

All data that is ingested and used by Adobe Experience Platform is stored in the [!DNL Data Lake], a highly granular data store containing all data managed by [!DNL Platform], regardless of origin or file format. All data persisted in the [!DNL Data Lake] is encrypted, stored, and managed in an isolated [!DNL Microsoft Azure Data Lake] Storage account that is unique to your organization.

The following process flow diagram illustrates how data is ingested, processed, encrypted, and persisted by [!DNL Experience Platform]:

![](images/data-protection/flow.png)

For details on how data at rest is encrypted in [!DNL Data Lake Storage], see the document on [data encryption in Azure Data Lake Storage](https://docs.microsoft.com/JA-JP/azure/data-lake-store/data-lake-store-encryption). For information on how data at rest is encrypted in [!DNL Cosmos DB], see the document on [data encryption in Azure Cosmos DB](https://docs.microsoft.com/JA-JP/azure/cosmos-db/database-encryption-at-rest).