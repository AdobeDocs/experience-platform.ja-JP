---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;fields;schemas;Schemas;poi;interaction;point of interest;point-of-interest;datatype;data-type;data type;
solution: Experience Platform
title: 目標地点インタラクションデータタイプ
topic: overview
description: このドキュメントでは、目標地点インタラクションXDMデータ型の概要を説明します。
translation-type: tm+mt
source-git-commit: 032adc72db7f094b268f14e8f7d48810830a84e4
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 3%

---


# [!UICONTROL 目標地点インタラクション] ・データ型

[!UICONTROL 目標地点インタラクション] (POI)は、モバイルデバイスが範囲内にあるときにID情報をモバイルアプリケーションに通信するワイヤレスデバイスを表す標準のXDMデータ型です。

<img src="../images/data-types/poi-interaction.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `poiDetail` | [[!UICONTROL 目標地点の詳細]](./poi-details.md) | イベントの原因となったPOIの詳細を説明します。 |
| `poiEntries` | オブジェクト | ユーザーがPOIに入った回数を示します。 次の2つのプロパティが含まれます。 <ul><li>`id`:メジャーの一意の識別子。</li><li>`value`:メジャーの定量化可能な値。</li></ul> |
| `poiExits` | オブジェクト | ユーザーがPOIから退出した回数を示します。 次の2つのプロパティが含まれます。 <ul><li>`id`:メジャーの一意の識別子。</li><li>`value`:メジャーの定量化可能な値。</li></ul> |

データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/poi-interaction.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/poi-interaction.schema.json)
