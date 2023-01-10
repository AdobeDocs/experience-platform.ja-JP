---
keywords: Experience Platform；ホーム；人気の高いトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；Web ページの詳細；データ型；データ型；データ型；web ページ
solution: Experience Platform
title: Experience Channel データタイプ
description: このドキュメントでは、エクスペリエンスチャネルエクスペリエンスデータモデル (XDM) データタイプの概要を説明します。
exl-id: 209654f7-0bde-439a-989c-ce2e41599105
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 22%

---

# [!UICONTROL Experience チャネル] データタイプ

[!UICONTROL Experience チャネル] は、エクスペリエンスチャネルを記述する標準のエクスペリエンスデータモデル (XDM) データ型です。 エクスペリエンスチャネルは、デジタルエクスペリエンスの利用方法またはパスを表します。

複数のエクスペリエンスチャネルがあり、それぞれにコンテンツの配信方法、顧客インタラクションの観測方法、データの収集方法に関する制約があります。 チャネル内では、エクスペリエンスを特定の場所に配信できます。 チャネル内に存在するロケーションの場所とタイプは、チャネルによって異なります。

![](../images/data-types/experience-channel.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_id` | 文字列 | チャネルを一意に識別する ID。 特定のエクスペリエンスチャネルごとに定数が定義されます `@id`. |
| `_type` | 文字列 | 類似のプロパティを持つチャネルに対して、大まかな分類ラベルを提供します。 |
| `contentTypes` | 文字列の配列 | このチャネルが配信できるコンテンツタイプ。 |
| `locationTypes` | 文字列の配列 | このチャネルが構成し、コンテンツを配信できる場所（仮想場所）のタイプ。 |
| `mediaAction` | 文字列 | エクスペリエンスイベントメディアアクションを表します（該当する場合）。 |
| `mediaType` | 文字列 | メディアタイプが有料か、所有か、獲得かを表します。 |
| `metricTypes` | 文字列の配列 | このチャネルで収集できる指標。 |
| `mode` | 文字列 | このチャネルでエクスペリエンスがどのように配信されるか。 |
| `typeAtSource` | 文字列 | チャネルのカスタム名。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/channels/channel.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/channels/channel.schema.json)
