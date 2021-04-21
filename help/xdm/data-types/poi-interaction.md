---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；ポイント；インタラクション；目標点；データ型；データ型；
solution: Experience Platform
title: 目標地点インタラクションデータタイプ
topic-legacy: overview
description: このドキュメントでは、目標地点インタラクションXDMデータ型の概要を説明します。
exl-id: 398f56d9-1802-458d-b565-4096beb5b014
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 3%

---

# [!UICONTROL 目標地点] インタラクションデータ型

[!UICONTROL 目標地点] インタラクションは、モバイルデバイスが範囲内にあるときにID情報をモバイルアプリケーションに通信するワイヤレスデバイスを表す標準のXDMデータ型です。

<img src="../images/data-types/poi-interaction.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `poiDetail` | [[!UICONTROL 目標地点の詳細]](./poi-details.md) | イベントの原因となったPOIの詳細を説明します。 |
| `poiEntries` | オブジェクト | ユーザーがPOIに入った回数を示します。 次の2つのプロパティが含まれます。 <ul><li>`id`:メジャーの一意の識別子。</li><li>`value`:メジャーの定量化可能な値。</li></ul> |
| `poiExits` | オブジェクト | ユーザーがPOIから退出した回数を示します。 次の2つのプロパティが含まれます。 <ul><li>`id`:メジャーの一意の識別子。</li><li>`value`:メジャーの定量化可能な値。</li></ul> |

データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/poi-interaction.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/poi-interaction.schema.json)
