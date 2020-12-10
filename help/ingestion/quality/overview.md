---
keywords: Experience Platform;home;popular topics;Data quality;quality;Quality;Supported validation;Validation;supported validation;
solution: Experience Platform
title: データ取り込みの品質
topic: overview
description: 次のドキュメントでは、Adobe Experience Platformでのバッチおよびストリーミング取り込みでサポートされているチェックおよび検証動作の概要を示します。
translation-type: tm+mt
source-git-commit: cfdaf72b7f4bf190877006ccd4cc6a7fd014adc2
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 74%

---


# Adobe Experience Platform のデータ品質

Adobe Experience Platform では、バッチまたはストリーミングの取り込みによりアップロードされたすべてのデータの完全性、正確性、一貫性が保証されています。The following document provides a summary of the supported checks and validation behaviors for batch and streaming ingestion in [!DNL Experience Platform].

## サポートされるチェック

|   | バッチの取り込み | ストリーミングの取り込み |
| ------ | --------------- | ------------------- |
| データタイプのチェック | ○ | ○ |
| 列挙のチェック | ○ | ○ |
| 範囲のチェック（最小、最大） | ○ | ○ |
| 必須フィールドのチェック | ○ | ○ |
| パターンのチェック | × | ○ |
| 書式のチェック | × | ○ |

## サポートされる検証ビヘイビアー

Both batch and streaming ingestion prevent failed data from going downstream by moving bad data for retrieval and analysis in [!DNL Data Lake]. バッチおよびストリーミングの取り込みでは、データに対して次の検証が実行されます。

### バッチの取り込み

バッチの取り込みでは、次の検証が実行されます。

| 検証領域 | 説明 |
| --------------- | ----------- |
| スキーマ | スキーマが空&#x200B;**ではなく**、`"meta:immutableTags": ["union"]` のように union スキーマへの参照が含まれていることを確認します。 |
| `identityField` | 有効な ID 記述子がすべて定義されていることを確認します。 |
| `createdUser` | バッチを取り込んだユーザーにバッチの取り込みが許可されていることを確認します。 |

### ストリーミングの取り込み

ストリーミングの取り込みでは、次の検証が実行されます。

| 検証領域 | 説明 |
| --------------- | ----------- |
| スキーマ | スキーマが空&#x200B;**ではなく**、`"meta:immutableTags": ["union"]` のように union スキーマへの参照が含まれていることを確認します。 |
| `identityField` | 有効な ID 記述子がすべて定義されていることを確認します。 |
| JSON | JSON が有効であることを確認します。 |
| IMS 組織 | 表示される IMS 組織が有効な組織であることを確認します。 |
| ソース名 | データソースの名前が指定されていることを確認します。 |
| データセット | データセットが指定され有効になっていること、さらに削除されていないことを確認します。 |
| ヘッダー | ヘッダーが指定され、有効になっていることを確認します。 |

More information about how [!DNL Platform] monitors and validates data can be found in the [monitoring data flows documentation](./monitor-data-ingestion.md).
