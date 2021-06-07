---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；Webページの詳細；データ型；データ型；データ型；Webページ
solution: Experience Platform
title: エクスペリエンスチャネルのデータタイプ
topic-legacy: overview
description: このドキュメントでは、エクスペリエンスチャネルエクスペリエンスデータモデル(XDM)データタイプの概要を説明します。
source-git-commit: b9168052174c250810e59e403cb77419d510df3b
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 22%

---

# [!UICONTROL エクスペリエ] ンスチャネルデータタイプ

[!UICONTROL エクスペリエ] ンスチャネルは、エクスペリエンスチャネルを記述する標準のエクスペリエンスデータモデル(XDM)データ型です。エクスペリエンスチャネルは、デジタルエクスペリエンスの消費方法またはパスを表します。

複数のエクスペリエンスチャネルがあり、それぞれにコンテンツの配信方法、顧客インタラクションの観察方法、データの収集方法に関する制約が異なります。 チャネル内で、エクスペリエンスを特定の場所に配信できます。 チャネル内に存在する場所の場所とタイプは、チャネルによって異なります。

![](../images/data-types/experience-channel.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_id` | 文字列 | チャネルを一意に識別するID。 特定のエクスペリエンスチャネルごとに定数`@id`が定義されます。 |
| `_type` | 文字列 | 類似のプロパティを持つチャネルの大まかな分類ラベルを提供します。 |
| `contentTypes` | 文字列の配列 | このチャネルが配信できるコンテンツタイプ。 |
| `locationTypes` | 文字列の配列 | このチャネルが構成し、コンテンツを配信できる場所（仮想場所）のタイプ。 |
| `mediaAction` | 文字列 | 該当する場合は、エクスペリエンスイベントメディアアクションを説明します。 |
| `mediaType` | 文字列 | メディアタイプが有料、所有、有料かを示します。 |
| `metricTypes` | 文字列の配列 | このチャネルで収集できる指標。 |
| `mode` | 文字列 | このチャネルでエクスペリエンスがどのように配信されるか。 |
| `typeAtSource` | 文字列 | チャネルのカスタム名。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/channels/channel.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/channels/channel.schema.json)
