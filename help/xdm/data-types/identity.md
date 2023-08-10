---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；ID；データ型；データ型；
solution: Experience Platform
title: ID データタイプ
description: このドキュメントでは、ID XDM データタイプの概要を説明します。
exl-id: fb02b6b4-255b-442f-895c-600022231a1c
source-git-commit: b66a50e40aaac8df312a2c9a977fb8d4f1fb0c80
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 10%

---

# [!UICONTROL ID] データタイプ

[!UICONTROL ID] は標準の XDM データ型で、デジタルエクスペリエンスとやり取りしている人を明確に区別するために使用されます。 ID は ID プロバイダーによって確立され、ID プロバイダー自体は `namespace` 属性。 次の期間内 `namespace`の場合、ID は一意です。

<img src="../images/data-types/identity.png" width="550" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `namespace` | オブジェクト | 単一の文字列フィールド (`code`) は、指定した `id` 属性。 |
| `authenticatedState` | 文字列 | 観測されたエクスペリエンスイベントの時点での、この ID の認証状態。 詳しくは、 [付録](#authenticatedState) を参照してください。 |
| `id` | 文字列 | 関連する名前空間での消費者の ID。 |
| `primary` | ブール値 | これが個人のプライマリ ID であるかどうかを示します。 各個人が持つことができるプライマリ ID は 1 つだけです。 |
| `xid` | 文字列 | 存在する場合、この値は、すべての名前空間内のすべての名前空間スコープ識別子全体で一意の名前空間間識別子を表します。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/identity.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/identity.schema.json)

## 付録

次の節では、 [!UICONTROL ID] データタイプ。

## authenticatedState に指定できる値 {#authenticatedState}

次の表に、 `authenticatedState` そしてそれに関連する意味も

| 値 | 説明 |
| --- | --- |
| `ambiguous` | 認証状態があいまいです。 |
| `authenticated` | ユーザーは、イベント監視時に有効だったログインまたは類似のアクションによって識別されました。 |
| `loggedOut` | ユーザーは、以前のある時点でのログインアクションによって識別されましたが、イベント監視時にはログインされませんでした。 |
