---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；フィールド；スキーマ；スキーマ；ID；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: Id データタイプ
description: ID XDM データタイプについて説明します。
exl-id: fb02b6b4-255b-442f-895c-600022231a1c
source-git-commit: 1d1224b263b55b290d2cac9c07dfd1b852c4cef5
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 11%

---

# [!UICONTROL ID] データタイプ

[!UICONTROL ID] は、デジタルエクスペリエンスとやり取りしている人物を明確に区別するために使用される標準の XDM データタイプです。 ID は、ID プロバイダーによって確立され、それ自体が `namespace` 属性で参照されます。 各 `namespace` 内で、ID は一意です。

![](../images/data-types/identity.png){width=550}

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `namespace` | オブジェクト | 指定された `id` 属性に関連付けられた名前空間を示す、単一の文字列フィールド（`code`）を含むオブジェクト。 |
| `authenticatedState` | 文字列 | 観測されたエクスペリエンスイベントの時点での、この ID の認証状態。 使用できる値と定義については、[&#x200B; 付録 &#x200B;](#authenticatedState) を参照してください。 |
| `id` | 文字列 | 関連する名前空間でのコンシューマーの ID。 |
| `primary` | ブール値 | これが個人のプライマリ ID であるかどうかを示します。 各個人は、1 つのプライマリ ID のみを持つことができます。 |
| `xid` | 文字列 | 存在する場合、この値は、すべての名前空間内のすべての名前空間スコープ識別子全体で一意の名前空間間識別子を表します。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/identity.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/identity.schema.json)

## 付録

次の節では、[!UICONTROL ID] データタイプに関する追加情報を示します。

## authenticatedState の許容値 {#authenticatedState}

次の表に、`authenticatedState` の許容値と関連する意味の概要を示します。

| 値 | 説明 |
| --- | --- |
| `ambiguous` | 認証状態があいまいです。 |
| `authenticated` | イベントの監視時に有効なログインなどのアクションによって、ユーザーが識別された。 |
| `loggedOut` | ユーザーは以前の時点でログインアクションによって識別されましたが、イベントの監視時にはログインしていませんでした。 |
