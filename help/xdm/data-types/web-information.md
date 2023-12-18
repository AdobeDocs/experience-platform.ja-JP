---
keywords: Experience Platform；ホーム；人気の高いトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；Web ページの詳細；データ型；データ型；データ型；web ページ
solution: Experience Platform
title: Web 情報データタイプ
description: Web 情報エクスペリエンスデータモデル (XDM) データタイプについて説明します。
exl-id: bfb00835-5908-4baf-af2a-6d845710e340
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 2%

---

# [!UICONTROL Web 情報] データタイプ

[!UICONTROL Web 情報] は標準の Experience Data Model(XDM) データ型で、World Wide Web チャネルに固有の Experience Event を介して記録される情報（ページ上のインタラクションに関連する Web ページ、リファラー、リンクなど）を記述します。

![](../images/data-types/web-information.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `webInteraction` | [[!UICONTROL Web インタラクション]](./web-interaction.md) | インタラクションに対応する Web リンクまたは URL に関する詳細を説明します。 |
| `webPageDetails` | [[!UICONTROL Web ページの詳細]](./webpage-details.md) | Web インタラクションが発生した Web ページに関する詳細を説明します。 |
| `webReferrer` | [!UICONTROL オブジェクト] | Web インタラクションのリファラーを表します。これは、現在の Web インタラクションが記録される直前に訪問者が来た URL です。 次のサブプロパティが含まれます。 <ul><li>`URL`：リファラー URL。</li><li>`type`：リファラータイプ。</li></ul> |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/webinfo.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/webinfo.schema.json)
