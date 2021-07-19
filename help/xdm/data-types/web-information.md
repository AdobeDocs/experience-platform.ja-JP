---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；Webページの詳細；データ型；データ型；データ型；Webページ
solution: Experience Platform
title: Web情報データ型
topic-legacy: overview
description: このドキュメントでは、Web情報エクスペリエンスデータモデル(XDM)データタイプの概要を説明します。
source-git-commit: 12c3f440319046491054b3ef3ec404798bb61f06
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 4%

---

# [!UICONTROL Web情] 報データ型

[!UICONTROL Web情] 報は、World Wide Webチャネルに固有のエクスペリエンスイベントを介して記録される情報（ページ上のインタラクションに関連するWebページ、リファラー、リンクなど）を記述する、標準のExperience Data Model(XDM)データ型です。

![](../images/data-types/web-information.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `webInteraction` | [[!UICONTROL Webインタラクション]](./web-interaction.md) | インタラクションに対応するWebリンクまたはURLの詳細を説明します。 |
| `webPageDetails` | [[!UICONTROL Webページの詳細]](./webpage-details.md) | Webインタラクションが発生したWebページに関する詳細について説明します。 |
| `webReferrer` | [!UICONTROL Object] | Webインタラクションのリファラーを示します。これは、現在のWebインタラクションが記録される直前に訪問者が来たURLです。 次のサブプロパティが含まれます。 <ul><li>`URL`:リファラーURL。</li><li>`type`:リファラータイプ。</li></ul> |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/webinfo.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/webinfo.schema.json)
