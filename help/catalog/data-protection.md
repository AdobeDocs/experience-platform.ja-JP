---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformでのデータ保護
topic: data protection
translation-type: tm+mt
source-git-commit: edf7cef0991ceef0465d5c1d750bd1885754f716

---


# Adobe Experience Platformでのデータ保護

Adobe Experience Platformで取り込まれ、使用されるすべてのデータは、接触チャネルやファイル形式に関係なく、Platformで管理されるすべてのデータを含む詳細なデータストアであるData Lakeに保存されます。 Data Lakeに保存されるすべてのデータは、暗号化され、保存され、組織に固有の独立したMicrosoft Azure Data Lakeストレージアカウントで管理されます。

次のプロセスフロー図は、データがエクスペリエンスプラットフォームで取り込まれ、処理、暗号化され、保持される方法を示しています。

![](images/data-protection/flow.png)

Data Lakeストレージでの保存データの暗号化方法の詳細については、Azure Data Lakeストレージのデー [タ暗号化のドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-encryption)。 Cosmos DBでの保存データの暗号化方法について詳しくは、Azure Cosmos DBでのデータの暗号化に関す [るドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/cosmos-db/database-encryption-at-rest)。