---
keywords: Experience Platform；ホーム；人気のあるトピック；カタログ；データ保護；暗号化データレーク
solution: Experience Platform
title: Adobe Experience Platform でのデータ保護
topic: data protection
description: データレイクに保持されるすべてのデータは、組織に固有の独立した Azure Data Lake Storage アカウントで暗号化、保存、管理されます。次のプロセスフロー図は、データがExperience Platformによって取り込まれ、処理、暗号化され、持続される方法を示しています。
translation-type: tm+mt
source-git-commit: 14f99c23cd82894fee5eb5c4093b3c50b95c52e8
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 27%

---


# Adobe Experience Platform でのデータ保護

Adobe Experience Platformが取り込んで使用するすべてのデータは、[!DNL Platform]が管理するすべてのデータを含む非常に詳細なデータストア[!DNL Data Lake]に保存されます。このデータストアは、接触チャネルやファイル形式に関係なく、 [!DNL Data Lake]に保存されるすべてのデータは、お客様の組織に固有の独立した[!DNL Microsoft Azure Data Lake]ストレージアカウントで暗号化、保存、管理されます。

次のプロセスフロー図は、データが[!DNL Experience Platform]によって取り込まれ、処理、暗号化され、持続される方法を示しています。

![](images/data-protection/flow.png)

[!DNL Data Lake Storage]での保存データの暗号化方法の詳細については、Azure Data Lakeストレージ](https://docs.microsoft.com/JA-JP/azure/data-lake-store/data-lake-store-encryption)の[データ暗号化のドキュメントを参照してください。 [!DNL Cosmos DB]での保存データの暗号化方法について詳しくは、Azure Cosmos DB](https://docs.microsoft.com/JA-JP/azure/cosmos-db/database-encryption-at-rest)の[データ暗号化のドキュメントを参照してください。