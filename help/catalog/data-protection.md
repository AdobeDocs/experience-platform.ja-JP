---
keywords: Experience Platform;ホーム;人気トピック;カタログ;データ保護;暗号化データレイク
solution: Experience Platform
title: Adobe Experience Platform でのデータ保護
topic-legacy: data protection
description: データレイクに保持されるすべてのデータは、組織に固有の独立した Azure Data Lake Storage アカウントで暗号化、保存、管理されます。次のプロセスフロー図は、Experience Platform でのデータの取得、処理、暗号化、保持の仕組みを示しています。
exl-id: 184b2b2d-8cd7-4299-83f8-f992f585c336
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 100%

---

# Adobe Experience Platform でのデータ保護

Adobe Experience Platform で取得され、使用されるすべてのデータは、出所やファイル形式に関係なく、[!DNL Platform]によって管理されるすべてのデータを含む非常にきめ細かいデータストアである[!DNL Data Lake]に保存されます。[!DNL Data Lake] に保持されるすべてのデータは、組織に固有の分離された[!DNL Microsoft Azure Data Lake]ストレージアカウントで暗号化、保存、および管理されます。

次のプロセスフロー図は、データが[!DNL Experience Platform]によって取得され、処理、暗号化、保持される仕組みを示しています。

![](images/data-protection/flow.png)

保存データの [!DNL Data Lake Storage]での暗号化の仕組みについて詳しくは、[Azure Data Lake ストレージのデータ暗号化](https://docs.microsoft.com/ja-jp/azure/data-lake-store/data-lake-store-encryption)のドキュメントを参照してください。保存データの [!DNL Cosmos DB] での暗号化の仕組みについては、[Azure Cosmos DB でのデータ暗号化](https://docs.microsoft.com/ja-jp/azure/cosmos-db/database-encryption-at-rest)に関するドキュメントを参照してください。
