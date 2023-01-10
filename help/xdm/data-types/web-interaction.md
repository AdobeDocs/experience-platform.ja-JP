---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；Web インタラクション；データ型；データ型；データ型；
solution: Experience Platform
title: Web インタラクションデータタイプ
description: このドキュメントでは、Web インタラクションエクスペリエンスデータモデル (XDM) データタイプの概要を説明します。
exl-id: 772d96c5-9fa3-4fed-8b38-16b8e7101743
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 5%

---

# [!UICONTROL Web インタラクション] データタイプ

[!UICONTROL Web インタラクション] は標準の Experience Data Model(XDM) データ型で、最初のページ読み込みが完了した後に Web ページで発生したインタラクションに関する情報を記述します。 これは、単一ページ Web アプリ (SPA) などの新しいページ読み込みをトリガーしない、リッチ Web アプリケーションでのインタラクションの記録を目的としています。

<img src="../images/data-types/web-interaction.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `linkClicks` | [[!UICONTROL 測定]](./measure.md) | Web リンクのクリックを追跡する指標です。 |
| `URL` | 文字列 | この Web インタラクションに使用される実際のリンクまたは URL。 |
| `name` | 文字列 | この Web リンクに使用される基準となる名前。 これは分類の目的で使用されます。 |
| `type` | 文字列 | リンクタイプ。 このプロパティは、次の列挙値のいずれかと等しい必要があります。 <li> `download` </li> <li> `exit` </li> <li> `other` </li> |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/webinteraction.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/webinteraction.schema.json)
