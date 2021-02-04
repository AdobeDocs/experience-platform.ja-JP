---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ;ID；データ型；データ型；
solution: Experience Platform
title: IDデータタイプ
topic: overview
description: このドキュメントでは、ID XDMデータ型の概要を説明します。
translation-type: tm+mt
source-git-commit: f5bddb39c16eb25e85297f56e331d3aa51510eb9
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 10%

---


#  Identitydata type

 Identityは、デジタルエクスペリエンスと対話するユーザーを明確に区別するために使用される、標準のXDMデータ型です。IDはIDプロバイダによって確立され、プロバイダ自身は`namespace`属性で参照されます。 各`namespace`内でIDは一意です。

<img src="../images/data-types/identity.png" width="550" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `namespace` | オブジェクト | 1つの文字列フィールド(`code`)を含むオブジェクト。指定した`id`属性に関連付けられている名前空間を示します。 |
| `authenticatedState` | 文字列 | 監視エクスペリエンスイベント時の、このIDの認証状態。 使用できる値と定義については、[付録](#authenticatedState)を参照してください。 |
| `id` | 文字列 | 関連名前空間内のコンシューマーのID。 |
| `primary` | Boolean | これが個人の主要IDであるかどうかを示します。 各個人が持つことのできる主IDは1つだけです。 |
| `xid` | 文字列 | 存在する場合、この値は、すべての名前空間内のすべての名前空間スコープ識別子全体で一意の名前空間間識別子を表します。 |

Mixinの詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/identity.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/identity.schema.json)

## 付録

次の節では、[!UICONTROL ID]データ型に関する追加情報を示します。

## authenticatedState {#authenticatedState}に指定できる値

次の表に、`authenticatedState`に使用できる値とその関連する意味を示します。

| 値 | 説明 |
| --- | --- |
| `ambiguous` | 認証状態があいまいです。 |
| `authenticated` | ユーザは、イベント監視の際に有効だったログインまたは類似のアクションによって識別されました。 |
| `loggedOut` | ユーザーは、以前のある時点でログインアクションによって識別されましたが、イベント監視の時点ではログインされませんでした。 |