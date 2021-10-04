---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；Web ページの詳細；データ型；データ型；データ型；Web ページ
solution: Experience Platform
title: Web 情報データ型
topic-legacy: overview
description: このドキュメントでは、Web 情報エクスペリエンスデータモデル (XDM) データタイプの概要を説明します。
exl-id: bfb00835-5908-4baf-af2a-6d845710e340
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 4%

---

# [!UICONTROL Web 情] 報データ型

[!UICONTROL Web 情] 報は、World Wide Web チャネルに固有のエクスペリエンスイベントを介して記録される情報（ページ上のインタラクションに関連する Web ページ、リファラー、リンクなど）を記述する標準の Experience Data Model(XDM) データ型です。

![](../images/data-types/web-information.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `webInteraction` | [[!UICONTROL Web インタラクション]](./web-interaction.md) | インタラクションに対応する Web リンクまたは URL の詳細を説明します。 |
| `webPageDetails` | [[!UICONTROL Web ページの詳細]](./webpage-details.md) | Web インタラクションが発生した Web ページに関する詳細について説明します。 |
| `webReferrer` | [!UICONTROL Object] | Web インタラクションの転送者を示します。これは、現在の Web インタラクションが記録される直前に訪問者が来た URL です。 次のサブプロパティが含まれます。 <ul><li>`URL`:リファラー URL。</li><li>`type`:リファラータイプ。</li></ul> |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/webinfo.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/webinfo.schema.json)
