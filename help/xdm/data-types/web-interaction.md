---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；Web インタラクション；データ型；データ型；
solution: Experience Platform
title: Web インタラクションデータタイプ
topic-legacy: overview
description: このドキュメントでは、Web インタラクションエクスペリエンスデータモデル (XDM) のデータタイプの概要を説明します。
exl-id: 772d96c5-9fa3-4fed-8b38-16b8e7101743
source-git-commit: 12c3f440319046491054b3ef3ec404798bb61f06
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 5%

---

# [!UICONTROL Web インタラク] ションデータ型

[!UICONTROL Web イン] タラクションは、最初のページ読み込みが完了した後に Web ページで発生したインタラクションに関する情報を記述する、標準的な Experience Data Model(XDM) データ型です。これは、単一ページ Web アプリケーション (SPA) などの新しいページ読み込みをトリガーしない、リッチ Web アプリケーションでのインタラクションの記録を目的としています。

<img src="../images/data-types/web-interaction.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `linkClicks` | [[!UICONTROL 測定]](./measure.md) | Web リンクのクリックを追跡する測定。 |
| `URL` | 文字列 | この Web インタラクションで使用される実際のリンクまたは URL。 |
| `name` | 文字列 | この Web リンクに使用する規範的な名前。 これは分類の目的で使用されます。 |
| `type` | 文字列 | リンクタイプ。 このプロパティは、次の列挙値のいずれかと等しい必要があります。 <li> `download` </li> <li> `exit` </li> <li> `other` </li> |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/webinteraction.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/webinteraction.schema.json)
