---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；Web ページの詳細；データ型；データ型；データ型；Web ページ
solution: Experience Platform
title: エクスペリエンスチャネルのデータタイプ
topic-legacy: overview
description: このドキュメントでは、エクスペリエンスチャネルエクスペリエンスデータモデル (XDM) データタイプの概要を説明します。
exl-id: 209654f7-0bde-439a-989c-ce2e41599105
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 22%

---

# [!UICONTROL エクスペリエン] スチャネルデータタイプ

[!UICONTROL エクスペリエ] ンスチャネルは、エクスペリエンスチャネルを説明する標準のエクスペリエンスデータモデル (XDM) データ型です。エクスペリエンスチャネルは、デジタルエクスペリエンスの利用方法またはパスを表します。

複数のエクスペリエンスチャネルがあり、それぞれにコンテンツの配信方法、顧客のインタラクションの観察方法、データの収集方法に関する制約が異なります。 チャネル内で、エクスペリエンスを特定の場所に配信できます。 チャネル内に存在する場所の場所とタイプは、チャネルによって異なります。

![](../images/data-types/experience-channel.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_id` | 文字列 | チャネルを一意に識別する ID。 特定のエクスペリエンスチャネルごとに定数 `@id` が定義されます。 |
| `_type` | 文字列 | 類似のプロパティを持つチャネルの大まかな分類ラベルを提供します。 |
| `contentTypes` | 文字列の配列 | このチャネルが配信できるコンテンツタイプ。 |
| `locationTypes` | 文字列の配列 | このチャネルが構成し、コンテンツを配信できる場所（仮想場所）のタイプ。 |
| `mediaAction` | 文字列 | 該当する場合は、エクスペリエンスイベントメディアアクションを説明します。 |
| `mediaType` | 文字列 | メディアの種類が有料か、所有か、または有料かを示します。 |
| `metricTypes` | 文字列の配列 | このチャネルで収集できる指標。 |
| `mode` | 文字列 | このチャネルでエクスペリエンスがどのように配信されるか。 |
| `typeAtSource` | 文字列 | チャネルのカスタム名。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/channels/channel.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/channels/channel.schema.json)
