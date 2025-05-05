---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；Web ページの詳細；データタイプ；データタイプ；データタイプ；Web ページ
solution: Experience Platform
title: Experience Channel データタイプ
description: Experience Channel Experience Data Model （XDM）データタイプについて説明します。
exl-id: 209654f7-0bde-439a-989c-ce2e41599105
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 24%

---

# [!UICONTROL &#x200B; エクスペリエンスチャネル &#x200B;] データタイプ

[!UICONTROL &#x200B; エクスペリエンスチャネル &#x200B;] は、エクスペリエンスチャネルを記述する標準の Experience Data Model （XDM）データタイプです。 エクスペリエンスチャネルは、デジタルエクスペリエンスの消費方法またはパスを表します。

複数のエクスペリエンスチャネルがあり、コンテンツが配信される方法、顧客インタラクションを監視する方法、データを収集する方法には、それぞれ異なる制約があります。 チャネル内では、エクスペリエンスを特定の場所に配信できます。 チャネル内に存在する場所の場所とタイプは、チャネルごとに異なります。

![](../images/data-types/experience-channel.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_id` | 文字列 | チャネルを一意に識別する ID。 それぞれの特定のエクスペリエンスチャネルは、一定の `@id` を定義します。 |
| `_type` | 文字列 | 類似のプロパティを持つチャネルの大まかな分類ラベルを提供します。 |
| `contentTypes` | 文字列の配列 | このチャネルが配信できるコンテンツタイプ。 |
| `locationTypes` | 文字列の配列 | このチャネルが構成し、コンテンツを配信できる場所（仮想場所）のタイプ。 |
| `mediaAction` | 文字列 | エクスペリエンスイベントメディアアクションを表します（該当する場合）。 |
| `mediaType` | 文字列 | メディアタイプがペイド、オウンドまたはアーンドのいずれかを表します。 |
| `metricTypes` | 文字列の配列 | このチャネルで収集できる指標。 |
| `mode` | 文字列 | このチャネルでエクスペリエンスがどのように配信されるか。 |
| `typeAtSource` | 文字列 | チャネルのカスタム名。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/channels/channel.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/channels/channel.schema.json)
