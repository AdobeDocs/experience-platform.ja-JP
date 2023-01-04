---
keywords: Experience Platform；ホーム；人気の高いトピック；PQL;pql；プロファイルクエリ言語
solution: Experience Platform
title: プロファイルクエリ言語 (PQL) の概要
topic-legacy: developer guide
description: このガイドでは、PQL の全般的な概要と、形式についてのガイドライン、PQL 式の例を示します。
exl-id: 4f7ab50e-89a3-42db-b74a-c6f2d86c9bcb
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 89%

---

# [!DNL Profile Query Language] (PQL) の概要

[!DNL Profile Query Language] (PQL) は [!DNL Experience Data Model] (XDM) 準拠のクエリ言語。 [!DNL Real-Time Customer Profile] データ。

このガイドでは、PQL の全般的な概要と、形式についてのガイドライン、PQL 式の例を示します。

## PQL クエリの形式

PQL クエリのシグネチャは次のとおりです。

```sql
({INPUT_PARAMETER_1}, {INPUT_PARAMETER_2}, ...) => {RESULT_TYPE}
```

入力パラメータは、ブールや文字列などの単純なプリミティブ、またはオブジェクト、配列、マップなどのより複雑な型にすることができます。

PQL 式の本文内で入力パラメーターを参照する方法は、次の 3 とおりあります。

### 最初のパラメーターへの暗黙の参照

次の例では、最初のパラメーターは常にコンテキスト内にあるので、直接それを指すプロパティ参照（`homeAddress`）を作成できます。

```sql
homeAddress.stateProvince = workAddress.stateProvince
```

### 最初のパラメーターへの明示的な参照

次の例では、`$1` は最初のパラメーターを参照しています。したがって、`$2` は 2 番目のパラメータを参照することになります。

```sql
$1.homeAddress.stateProvince = $1.homeAddress.stateProvince
```

### 名前付き変数の使用（ラムダ表記）

次の例では、`Profile` は変数名で、クエリの作成者が選択できます。

```sql
(Profile) => Profile.homeAddress.stateProvince = Profile.workAddress.stateProvince
```

## PQL リテラル

PQL では次のリテラル型をサポートしています。

| リテラル | 定義 | 例 |
| ------- | ---------- | ------- |
| 文字列 | 1 つ以上の文字で構成され、二重引用符で囲まれたデータタイプです。 | `"pizza"`、`"jobs"`、`"antidisestablishmentarianism"` |
| ブール | true か false のいずれかであるデータタイプです。 | `true`、`false` |
| 整数 | 整数を表すデータタイプです。正、負、ゼロのいずれかです。 | `-201`、`0`、`412` |
| 倍精度実数 | 任意の実数を表すデータ型です。正、負、ゼロのいずれかです。 | `-51.24`、`3.14`、`0.6942058` |
| 日付 | 年、月、日（整数パラメーター）に基づいて日付を作成するために使用できるデータ型です。`date(year, month, day)` という形式で記述します。 | `date(2020, 3, 14)` |
| 配列 | 他のリテラル値のグループとして構成されるデータ型です。複数の値を区切る場合は、角括弧で囲んでグループ化し、カンマで区切ります。<br> **注意**：配列内の項目のプロパティに直接アクセスすることはできません。したがって、配列内のプロパティにアクセスする必要がある場合、サポートされているメソッドは `select X from array where X.item = ...` です。<br> PQL には、プロファイルにリンクされたエクスペリエンスイベントの配列を指す予約語 `xEvent` があります。 | `[1, 4, 7]`、`["US", "CA"]` |
| 相対時間参照 | タイムスタンプおよび時間間隔の参照の形式設定に使用できる予約語。 <ul><li>now、today、yesterday、tomorrow</li><li>this、last、next</li><li>before、after、from</li><li>millisecond／milliseconds、second／seconds、minute／minutes、hour／hours、day／days、week／weeks、month／months、year／years、decade／decades、century／centuries、millennium／millennia</li></ul> | `X.timestamp occurs before today`、`X.timestamp occurs last month`、`X.timestamp occurs <= 3 days before now` |


## PQL 関数

サポートされている PQL 関数の各種カテゴリを次の表に示します。詳細を説明するドキュメントへのリンクも含まれています。

| カテゴリ | 定義 |
| -------- | ---------- |
| ブール | PQL 内にブール代数を実装するために使用します。これらの関数について詳しくは、[ブール関数のドキュメント](./boolean-functions.md)を参照してください。 |
| 比較 | 異なる PQL 要素間の比較に使用します。これらの関数について詳しくは、[比較関数のドキュメント](./comparison-functions.md)を参照してください。 |
| 配列、リスト、セット | 配列、リスト、セットの操作に使用されます。これらの関数について詳しくは、[配列関数、リスト関数、セット関数のドキュメント](./array-functions.md)を参照してください。 |
| マップ | マップの操作に使用します。これらの関数について詳しくは、[マップ関数のドキュメント](./map-functions.md)を参照してください。 |
| 文字列 | 文字列の操作に使用します。これらの関数について詳しくは、[文字列関数のドキュメント](./string-functions.md)を参照してください。 |
| オブジェクト | オブジェクトの操作に使用します。 これらの関数について詳しくは、 [オブジェクト関数ドキュメント](./object-functions.md). |
| 算術演算 | PQL 要素に対して基本的な算術演算を実行するために使用します。これらの関数について詳しくは、[算術演算関数のドキュメント](./arithmetic-functions.md)を参照してください |
| 集計 | 配列の結果を 1 つの結果に組み合わせるために使用します。これらの関数について詳しくは、[集計関数のドキュメント](./aggregation-functions.md)を参照してください。 |
| 日時 | 日付、時刻、日時の各オブジェクトと組み合わせて使用します。これらの関数について詳しくは、[日付／時刻関数のドキュメント](./datetime-functions.md)を参照してください。 |
| フィルター | 配列内のデータのフィルタリングに使用します。これらの関数について詳しくは、[フィルター関数のドキュメント](./filter-functions.md)を参照してください。 |
| 論理量指定子 | 配列内の条件をアサートするために使用します。詳しくは、[論理量指定子のドキュメント](./logical-quantifiers.md)を参照してください。 |
| その他 | 上記カテゴリのいずれにも当てはまらない関数は、[その他の関数のドキュメント](./misc-functions.md)を参照してください。 |

## 次の手順

これで、 [!DNL Profile Query Language]の場合は、セグメントを作成および変更する際に PQL を使用できます。 セグメント化について詳しくは、[セグメント化の概要](../home.md)を参照してください。
