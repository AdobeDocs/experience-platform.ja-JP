---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ;Webインタラクション；データ型；データ型；
solution: Experience Platform
title: Webインタラクションデータ型
topic-legacy: overview
description: このドキュメントでは、Webインタラクションエクスペリエンスデータモデル(XDM)のデータタイプの概要を説明します。
exl-id: 772d96c5-9fa3-4fed-8b38-16b8e7101743
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 4%

---

# [!UICONTROL Web] インタラクションデータ型

[!UICONTROL Web] インタラクションは、最初のページ読み込みが完了した後にWebページ上で発生したインタラクションに関する情報を記述する、標準のExperience Data Model(XDM)データ型です。単一ページのWebアプリ(SPA)など、新しいページ読み込みをトリガーしないリッチWebアプリケーションでのインタラクションの記録を目的としています。

<img src="../images/data-types/web-interaction.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `linkClicks` | [[!UICONTROL 測定]](./measure.md) | Webリンクのクリックを追跡する測定値。 |
| `URL` | 文字列 | このWebインタラクションで使用される実際のリンクまたはURLです。 |
| `name` | 文字列 | このWebリンクに使用する標準名です。 これは分類の目的で使用されます。 |
| `type` | 文字列 | リンクタイプ。 このプロパティは、次の列挙値のいずれかと等しい必要があります。 <li> `download` </li> <li> `exit` </li> <li> `other` </li> |

データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/web/webinteraction.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/web/webinteraction.schema.json)
