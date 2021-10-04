---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；ID；データ型；データ型；
solution: Experience Platform
title: ID データタイプ
topic-legacy: overview
description: このドキュメントでは、ID XDM データタイプの概要を説明します。
exl-id: fb02b6b4-255b-442f-895c-600022231a1c
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 11%

---

#  ID データ型

 ID は、デジタルエクスペリエンスとやり取りしている人を明確に区別するために使用される標準の XDM データ型です。ID は ID プロバイダーによって確立され、ID プロバイダー自体は `namespace` 属性で参照されます。 各 `namespace` 内で、ID は一意です。

<img src="../images/data-types/identity.png" width="550" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `namespace` | オブジェクト | 単一の文字列フィールド (`code`) を含むオブジェクト。指定した `id` 属性に関連付けられた名前空間を示します。 |
| `authenticatedState` | 文字列 | 観察されたエクスペリエンスイベントの時点での、この ID の認証状態。 指定できる値と定義については、[ 付録 ](#authenticatedState) を参照してください。 |
| `id` | 文字列 | 関連する名前空間の消費者の ID。 |
| `primary` | Boolean | この ID が個人のプライマリ ID であるかどうかを示します。 個々の個人が持つことができるプライマリ ID は 1 つだけです。 |
| `xid` | 文字列 | 存在する場合、この値は、すべての名前空間内のすべての名前空間スコープ識別子全体で一意の名前空間間識別子を表します。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/identity.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/identity.schema.json)

## 付録

次の節では、[!UICONTROL ID] データ型に関する追加情報を示します。

## authenticatedState に指定できる値 {#authenticatedState}

次の表に、`authenticatedState` の受け入れ可能な値とそれに関連する意味の概要を示します。

| 値 | 説明 |
| --- | --- |
| `ambiguous` | 認証状態があいまいです。 |
| `authenticated` | ユーザーは、イベント監視時に有効だったログインまたは類似のアクションによって識別されました。 |
| `loggedOut` | ユーザーは、以前の時点でのログインアクションによって識別されましたが、イベント監視時にはログインされませんでした。 |
