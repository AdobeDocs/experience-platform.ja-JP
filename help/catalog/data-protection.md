---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformでのデータ保護
topic: data protection
translation-type: tm+mt
source-git-commit: edf7cef0991ceef0465d5c1d750bd1885754f716
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---


# Adobe Experience Platformでのデータ保護

Adobe Experience Platformが取り込んで使用するすべてのデータは、接触チャネルやファイル形式に関係なく、Platformが管理するすべてのデータを含む詳細なデータストアであるData Lakeに保存されます。 Data Lakeに保存されるすべてのデータは、組織に固有の独立したMicrosoft Azure Data Lakeストレージアカウントで暗号化、保存、管理されます。

次のプロセスフロー図は、エクスペリエンスプラットフォームでデータを取り込み、処理、暗号化および保持する方法を示しています。

![](images/data-protection/flow.png)

Data Lakeストレージでの保存データの暗号化方法の詳細については、Azure Data Lakeストレージでの [データ暗号化のドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-encryption)。 Cosmos DBでの保存データの暗号化方法について詳しくは、Azure Cosmos DBの [データ暗号化に関するドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/cosmos-db/database-encryption-at-rest)。