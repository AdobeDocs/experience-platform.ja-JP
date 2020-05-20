---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: プロファイルクエリ言語(PQL)の概要
topic: developer guide
translation-type: tm+mt
source-git-commit: 902ba5efbb5f18a2de826fffd023195d804309cc
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 3%

---


# プロファイルクエリ言語(PQL)の概要

プロファイルクエリ言語(PQL)は、Experience Data Model(XDM)に準拠したクエリ言語です。この言語は、リアルタイム顧客プロファイルデータのセグメントクエリの定義と実行をサポートするように設計されています。

このガイドでは、PQLの一般的な概要を説明し、フォーマットのガイドラインとPQL式の例を示します。

## PQLクエリのフォーマット

PQLクエリには次のシグネチャがあります。

```sql
({INPUT_PARAMETER_1}, {INPUT_PARAMETER_2}, ...) => {RESULT_TYPE}
```

入力パラメーターは、ブール値や文字列などの単純なプリミティブ、またはオブジェクト、配列、マップなどのより複雑な型にすることができます。

PQL式の本体内で入力パラメータを参照する方法は **3通り** あります。

### 最初のパラメーターへの暗黙の参照

次の例では、最初のパラメーターは常にコンテキスト内にあるので、プロパティ参照(`homeAddress`)を直接作成できます。

```sql
homeAddress.stateProvince = workAddress.stateProvince
```

### 最初のパラメーターへの明示的な参照

次の例では、は最初のパラメーター `$1` を参照します。 その結果、は2番目 `$2` のパラメーターなどを参照します。

```sql
$1.homeAddress.stateProvince = $1.homeAddress.stateProvince
```

### 名前付き変数の使用（ラムダ表記を使用）

次の例では、 `Profile` は変数名で、クエリの作成者が選択できます。

```sql
(Profile) => Profile.homeAddress.stateProvince = Profile.workAddress.stateProvince
```

## PQLリテラル

PQLでは、次のリテラル型がサポートされます。

| リテラル | 定義 | 例 |
| ------- | ---------- | ------- |
| 文字列 | 重複の引用符で囲まれた文字で構成されるデータ型です。 | `"pizza"`, `"jobs"`, `"antidisestablishmentarianism"` |
| Boolean | trueまたはfalseのデータ型です。 | `true`、`false` |
| 整数 | 整数を表すデータ型です。 正、負、またはゼロのいずれかです。 | `-201`, `0`, `412` |
| 重複 | 任意の実数を表すデータ型です。 正、負、またはゼロのいずれかです。 | `-51.24`, `3.14`, `0.6942058` |
| 日付 | 年、月、日に基づく日付を整数のパラメーターとして作成するために使用できるデータ型です。 It is formatted as `date(year, month, day)` | `date(2020, 3, 14)` |
| 配列 | 他のリテラル値のグループとして構成されるデータ型です。 異なる値を区切るために、角括弧で囲み、カンマを使用します。 <br> **注意：** 配列内の項目のプロパティに直接アクセスすることはできません。 したがって、配列内のプロパティにアクセスする必要がある場合は、次のメソッドがサポートされ `select X from array where X.item = ...`ます。 <br> PQLは、プロファイルにリンクされた一連のエクスペリエンスイベント `xEvent` を参照する単語を予約します。 | `[1, 4, 7]`、`["US", "CA"]` |
| 相対時間参照 | タイムスタンプと時間間隔の参照を形成するために使用できる予約語です。 <ul><li>今日昨日明日</li><li>これ、最後、次</li><li>前、後、後</li><li>ミリ秒、秒、分、時間、日、週、月、年、10年、世紀/世紀、ミレニアム/ミレニアム</li></ul> | `X.timestamp occurs before today`, `X.timestamp occurs last month`, `X.timestamp occurs <= 3 days before now` |


## PQL関数

次の表に、サポートされるPQL関数の各カテゴリの概要を示します。詳細については、その他のドキュメントへのリンクも含まれています。

| カテゴリ | 定義 |
| -------- | ---------- |
| Boolean | PQL内にブール代数を実装するために使用します。 これらの関数について詳しくは、 [boolean functionsドキュメントーを参照してください](./boolean-functions.md)。 |
| 比較 | 異なるPQL要素間の比較に使用します。 これらの関数の詳細については、「 [比較関数」ドキュメントを参照してください](./comparison-functions.md)。 |
| アレイ、リスト、セット | 配列、リスト、セットの操作に使用します。 これらの関数の詳細は、 [配列、リスト、set functionsドキュメントに記載されています](./array-functions.md)。 |
| マップ | マップの操作に使用します。 これらの関数の詳細については、「 [map functionsドキュメント](./map-functions.md)」を参照してください。 |
| 文字列 | 文字列を操作するために使用します。 これらの関数について詳しくは、 [文字列関数ドキュメントーを参照してください](./string-functions.md)。 |
| 演算 | PQL要素に対して基本的な演算を実行するために使用します。 これらの関数の詳細については、 [演算関数ドキュメントを参照してください](./arithmetic-functions.md) |
| 集計 | 配列の結果を単数の結果に結合するために使用します。 集計関数の詳細は、 [集計関数ドキュメントを参照してください](./aggregation-functions.md)。 |
| 日時 | date、time、datetimeの各オブジェクトと組み合わせて使用します。 これらの関数について詳しくは、 [日付/時間関数ドキュメントを参照してください](./datetime-functions.md)。 |
| フィルター | 配列内のデータをフィルタするために使用します。 これらの関数の詳細については、「 [filter functionsドキュメント](./filter-functions.md)」を参照してください。 |
| 論理量指定子 | 配列内の条件をアサートするために使用します。 詳細は、 [論理量指定子のドキュメントで確認できます](./logical-quantifiers.md)。 |
| その他 | 上記のカテゴリのいずれにも当てはまらない関数は、 [その他の関数ドキュメントにあります](./misc-functions.md)。 |

## 次の手順

プロファイルクエリ言語の使い方を習得したので、セグメントの作成と変更時にPQLを使用できます。 セグメント化について詳しくは、 [セグメント化の概要を参照してください](../home.md)。