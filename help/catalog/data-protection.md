---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformでのデータ保護
topic: data protection
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---


# Adobe Experience Platformでのデータ保護

Adobe Experience Platformが取り込んで使用するすべてのデータは、接触チャネルやファイル形式に関係なく、によって管理されるすべてのデータを含む非常に詳細なデータストアに格納され [!DNL Data Lake][!DNL Platform]ます。 で保存されるすべてのデータ [!DNL Data Lake] は、組織に固有の独立した [!DNL Microsoft Azure Data Lake] ストレージアカウントで暗号化、保存、管理されます。

次のプロセスフロー図は、データを取り込み、処理、暗号化および持続させる方法を示してい [!DNL Experience Platform]ます。

![](images/data-protection/flow.png)

での保存データの暗号化方法の詳細につ [!DNL Data Lake Storage]いては、Azure Data Lakeストレージでの [データ暗号化に関するドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-encryption)。 での保存データの暗号化方法について詳し [!DNL Cosmos DB]くは、Azure Cosmos DBの [データ暗号化に関するドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/cosmos-db/database-encryption-at-rest)。