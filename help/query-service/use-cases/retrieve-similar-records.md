---
title: 上位関数を使用した類似レコードの取得
description: 類似性指標と類似性しきい値に基づいて、1 つ以上のデータセットから類似したレコードや関連するレコードを識別し、取得する方法について説明します。 このワークフローでは、意味のある関係や、異なるデータセット間の重複を強調表示することができます。
exl-id: 4810326a-a613-4e6a-9593-123a14927214
source-git-commit: 27eab04e409099450453a2a218659e576b8f6ab4
workflow-type: tm+mt
source-wordcount: '4031'
ht-degree: 3%

---

# 上位関数を使用して類似したレコードを取得

Data Distillerの高次の関数を使用して、様々な一般的な使用例を解決します。 1 つ以上のデータセットから類似したレコードや関連するレコードを識別および取得するには、このガイドで詳しく説明するように、フィルター、変換、減少の機能を使用します。 複雑なデータ型の処理に高次の関数を使用する方法については、 [配列とマップのデータ型の管理](../sql/higher-order-functions.md).

このガイドを使用して、特性や属性に大きな類似点がある様々なデータセットの製品を識別します。 この方法論は、データの重複排除、レコードのリンケージ、レコメンデーションシステム、情報の取得、テキスト分析などのソリューションを提供します。

このドキュメントでは、類似性結合を実装するプロセスについて説明します。このプロセスでは、Data Distillerの上位関数を使用して、データのセット間の類似性を計算し、選択した属性に基づいてフィルタリングします。 SQL コードスニペットと説明は、プロセスの各手順で提供されます。 このワークフローは、Data Distillerの上位関数を使用した Jaccard 類似性測定とトークン化を使用して、類似性結合を実装します。 次に、これらのメソッドは、類似性指標に基づいて、類似したレコードや関連するレコードを 1 つ以上のデータセットから識別し、取得するために使用されます。 プロセスの主なセクションは次のとおりです。 [高次関数を使用したトークン化](#data-transformation)、 [一意の要素のクロス結合](#cross-join-unique-elements)、 [Jaccard 類似性計算](#compute-the-jaccard-similarity-measure)、および [しきい値ベースのフィルタリング](#similarity-threshold-filter).

## 前提条件

このドキュメントに進む前に、次の概念について理解しておく必要があります。

- A **類似性結合** は、レコード間の類似性の測定に基づいて 1 つ以上のテーブルからレコードのペアを識別し、取得する操作です。 類似性結合の主な要件は次のとおりです。
   - **類似性指標**：類似性結合は、事前に定義された類似性指標または測定に基づいています。 このような指標には、Jaccard の類似性、コサインの類似性、編集距離などが含まれます。 指標は、データの特性と使用例に応じて異なります。 この指標は、2 つのレコードの類似性または類似性を定量化します。
   - **しきい値**：類似性しきい値は、結合結果に含まれるのに十分に類似していると見なされる 2 つのレコードを判断するために使用されます。 類似性スコアがしきい値を超えるレコードは、一致と見なされます。
- The **Jaccard 類似性** index（Jaccard の類似性測定）は、サンプルセットの類似性と多様性を測定するために使用される統計です。 積集合のサイズを、サンプルセットの和集合のサイズで割った値として定義します。 Jaccard 類似性測定は、0 から 1 の範囲です。 ゼロの Jaccard 類似性は、セット間の類似性を示さず、1 の Jaccard 類似性は、セットが同一であることを示します。
  ![Jaccard の類似性測定を示すベン図です。](../images/use-cases/jaccard-similarity.png)
- **上位関数** Data Distillerは、SQL ステートメント内でデータを直接処理および変換する、動的なインラインツールです。 これらの汎用的な機能により、特に [配列やマップなどの複雑な型の処理](../sql/higher-order-functions.md). クエリの効率を高め、変換を簡素化することで、上位の関数は、様々なビジネスシナリオでのより機敏な分析とより優れた意思決定に貢献します。

## はじめに

Data Distiller SKU は、Adobe Experience Platformデータに対して上位の機能を実行するために必要です。 Data Distiller SKU をお持ちでない場合、詳しくは、Adobeのカスタマーサービス担当者にお問い合わせください。

## 類似性の確立 {#establish-similarity}

この使用例では、後でフィルタリングのしきい値を設定するために使用できる、テキスト文字列間の類似性の測定基準が必要です。 この例では、Set A と Set B の製品は、2 つのドキュメント内の単語を表します。

Jaccard の類似性測定は、テキストデータ、分類データ、バイナリデータなど、様々なデータタイプに適用できます。 また、大きなデータセットの計算を計算する計算効率が高いので、リアルタイム処理やバッチ処理にも適しています。

製品セット A とセット B には、このワークフローのテストデータが含まれます。

- 製品セット A: `{iPhone, iPad, iWatch, iPad Mini}`
- 製品セット B: `{iPhone, iPad, Macbook Pro}`

製品セット A と B の間の Jaccard の類似性を計算するには、まず **積集合** 製品セットの（共通の要素）。 この場合、 `{iPhone, iPad}`. 次に、 **和集合** （すべての一意の要素）を含む ) 両方の製品セット。 この例では、 `{iPhone, iPad, iWatch, iPad Mini, Macbook Pro}`.

最後に、Jaccard の類似性の数式を使用します。 `J(A,B) = A∪B / A∩B` 類似性を計算します。

J =ジャカード距離 A =設定 1 B =設定 2

製品セット A と B の Jaccard 類似性は 0.4 です。これは、2 つのドキュメントで使用される単語間の類似性の程度を示しています。 2 つのセット間のこの類似性は、類似性結合の列を定義します。 これらの列は、テーブルに格納され、類似性計算の実行に使用される情報またはデータに関連付けられた特性を表します。

### 文字列の類似性を持つ一対の Jaccard 計算 {#pairwise-similarity}

文字列間の類似性をより正確に比較するには、一対の類似性を計算する必要があります。 一対の類似性は、高い次元のオブジェクトを、比較と分析のために小さな次元のオブジェクトに分割します。 これを行うには、テキストの文字列を小さな部分または単位（トークン）に分割します。 個々の文字、文字のグループ（音節など）、単語全体を指定できます。 類似性は、セット A の各要素とセット B の各要素との間のトークンのペアごとに計算されます。このトークン化は、データから引き出される分析および計算の比較、関係、インサイトの基礎となります。

この例では、一対の類似性の計算で、文字のバイグラム（2 文字のトークン）を使用して、Set A と Set B の製品のテキスト文字列の類似性の一致を比較します。バイグラムとは、特定のシーケンスまたはテキスト内の 2 つの項目または要素の連続したシーケンスです。 これを n グラムに一般化することができます。

この例では、大文字と小文字が区別されず、そのスペースは考慮されないと仮定します。 これらの条件に従って、Set A と Set B は次のバイグラムを持ちます。

製品セット A のバイグラム：

- iPhone(5):「ip」、「ph」、「ho」、「on」、「ne」
- iPad(3): &quot;ip&quot;、&quot;pa&quot;、&quot;ad&quot;
- iWatch (5): &quot;iw&quot;, &quot;wa&quot;, &quot;at&quot;, &quot;tc&quot;, &quot;ch&quot;
- iPad Mini(7): &quot;ip&quot;、&quot;pa&quot;、&quot;ad&quot;、&quot;dm&quot;、&quot;mi&quot;、&quot;in&quot;、&quot;ni&quot;

製品セット B バイグラム：

- iPhone(5):「ip」、「ph」、「ho」、「on」、「ne」
- iPad(3): &quot;ip&quot;、&quot;pa&quot;、&quot;ad&quot;
- Macbook Pro (9): &quot;Ma&quot;, &quot;ac&quot;, &quot;cb&quot;, &quot;bo&quot;, &quot;oo&quot;, &quot;ok&quot;, &quot;kp&quot;, &quot;pr&quot;, &quot;ro&quot;

次に、各ペアの Jaccard 類似性係数を計算します。

|                   | iPhone（セット B） | iPad（セット B） | Macbook Pro （セット B） |
|-------------------|----------------------------------------------|---------------------------------------------|-------------------------------------------|
| iPhone（セット A） | （積集合： 5、和集合： 5） = 5 / 5 = 1 | （積集合： 1，和集合： 7） =1 / 7 ≈ 0.14 | （積集合： 0、和集合： 14） = 0 / 14 = 0 |
| iPad（セット A） | （積集合： 1，和集合： 7） = 1 / 7 ≈ 0.14 | （積集合： 3、和集合： 3） = 3 / 3 = 1 | （積集合： 0、和集合： 12） = 0 / 12 = 0 |
| iWatch （セット A） | （積集合： 0、和集合： 8） = 0 / 8 = 0 | （積集合： 0、和集合： 8） = 0 / 8 = 0 | （積集合： 0、和集合： 8） = 0 / 8 =0 |
| iPad Mini （セット A） | （交差：1、和集合：11） = 1 / 11 ≈ 0.09 | （交点：3、和集合：7） = 3 / 7 ≈ 0.43 | （積集合： 0、和集合： 16） = 0 / 16 = 0 |

{style="table-layout:auto"}

## SQL でのテストデータの作成 {#create-test-data}

製品セットのテスト表を手動で作成するには、SQL CREATE TABLE 文を使用します。

```SQL {line-numbers="true"}
CREATE TABLE featurevector1 AS SELECT *
FROM (
    SELECT 'iPad' AS ProductName
    UNION ALL
    SELECT 'iPhone'
    UNION ALL
    SELECT 'iWatch'
     UNION ALL
    SELECT 'iPad Mini'
);
SELECT * FROM featurevector1;
```

以下の説明は、上記の SQL コードブロックの分類を示しています。

- 行 1: `CREATE TEMP TABLE featurevector1 AS`：この文は、という名前の一時テーブルを作成します。 `featurevector1`. 一時テーブルは通常、現在のセッション内でのみアクセス可能で、セッションの最後に自動的に削除されます。
- 行 1 および 2: `SELECT * FROM (...)`：コードのこの部分は、 `featurevector1` 表。
サブクエリ内で、複数の `SELECT` 文は、 `UNION ALL` コマンドを使用します。 各 `SELECT` 文は、 `ProductName` 列。
- 行 3: `SELECT 'iPad' AS ProductName`：値を持つ行を生成します `iPad` （内） `ProductName` 列。
- 行 5: `SELECT 'iPhone'`：値を持つ行を生成します `iPhone` （内） `ProductName` 列。

SQL 文は、次に示すようにテーブルを作成します。

|   | `ProductName` |
|---|---------------|
| 1 | iPad |
| 2 | iPhone |
| 3 | iWatch |
| 4 | iPad Mini |

{style="table-layout:auto"}

2 番目のフィーチャベクトルを作成するには、次の SQL ステートメントを使用します。

```SQL
CREATE TABLE featurevector2 AS SELECT *
FROM (
    SELECT 'iPad' AS ProductName
    UNION ALL
    SELECT 'iPhone'
    UNION ALL
    SELECT 'Macbook Pro'
);
SELECT * FROM featurevector2;
```

## データ変換 {#data-transformation}

この例では、複数のアクションを実行して、セットを正確に比較する必要があります。 まず、類似性測定に影響を与えないと想定されるので、空白はすべて特徴ベクトルから削除されます。 その後、特徴ベクトルに存在する重複は、計算処理を無駄にするので除去されます。 次に、特徴ベクトルから 2 文字（バイグラム）のトークンを抽出する。 この例では、重複していると見なされます。

>[!NOTE]
>
>説明用に、各ステップのフィーチャベクトルの横に、処理された列が追加されます。

次の節では、トークン化のプロセスを開始する前に、重複排除、空白の削除、小文字の変換などの前提条件となるデータ変換について説明します。

### 重複排除 {#deduplication}

次に、 `DISTINCT` 句を使用して重複を削除します。 この例では重複はありませんが、比較の精度を向上させるための重要な手順です。 必要な SQL を次に示します。

```SQL
SELECT DISTINCT(ProductName) AS featurevector1_distinct FROM featurevector1
SELECT DISTINCT(ProductName) AS featurevector2_distinct FROM featurevector2
```

### 空白の削除 {#whitespace-removal}

次の SQL 文では、空白がフィーチャベクトルから削除されます。 The `replace(ProductName, ' ', '') AS featurevector1_nospaces` クエリの一部は、 `ProductName` 列を `featurevector1` テーブルと `replace()` 関数に置き換えます。 The `REPLACE` 関数は、スペース (「 」) の出現箇所をすべて空の文字列 (「」) に置き換えます。 これにより、 `ProductName` 値。 結果は次のようにエイリアスされます。 `featurevector1_nospaces`.

```SQL
SELECT DISTINCT(ProductName) AS featurevector1_distinct, replace(ProductName, ' ', '') AS featurevector1_nospaces FROM featurevector1
```

結果を次の表に示します。

|   | featurevector1_distinct | featurevector1_nospaces |
|---|---|---|
| 1 | iPad Mini | iPadMini |
| 2 | iPad | iPad |
| 3 | iWatch | iWatch |
| 4 | iPhone | iPhone |

{style="table-layout:auto"}

2 番目の特徴ベクトルに対する SQL 文とその結果を以下に示します。

+++選択して展開します

```SQL
SELECT DISTINCT(ProductName) AS featurevector2_distinct, replace(ProductName, ' ', '') AS featurevector2_nospaces FROM featurevector2
```

結果は次のように表示されます。

|   | featurevector2_distinct | featurevector2_nospaces |
|---|---|---|
| 1 | iPad | iPad |
| 2 | Macbook Pro | MacbookPro |
| 3 | iPhone | iPhone |

{style="table-layout:auto"}

+++

### 小文字に変換 {#lowercase-conversion}

次に、SQL が改善され、製品名が小文字に変換され、空白が削除されます。 lower 関数 (`lower(...)`) が `replace()` 関数に置き換えます。 lower 関数は、変更された `ProductName` の値を小文字に変換します。 これにより、元の大文字と小文字に関係なく、値が小文字で表示されます。

```SQL
SELECT DISTINCT(ProductName) AS featurevector1_distinct, lower(replace(ProductName, ' ', '')) AS featurevector1_transform FROM featurevector1;
```

この文の結果は次のようになります。

|   | featurevector1_distinct | featurevector1_transform |
|---|---|---|
| 1 | iPad Mini | ipadmini |
| 2 | iPad | iPad |
| 3 | iWatch | iWatch |
| 4 | iPhone | iPhone |

{style="table-layout:auto"}

2 番目の特徴ベクトルに対する SQL 文とその結果を以下に示します。

+++選択して展開します

```SQL
SELECT DISTINCT(ProductName) AS featurevector2_distinct, lower(replace(ProductName, ' ', '')) AS featurevector2_transform FROM featurevector2
```

結果は次のように表示されます。

|   | featurevector2_distinct | featurevector2_transform |
|---|---|---|
| 1 | iPad | ipad |
| 2 | Macbook Pro | macbookpro |
| 3 | iPhone | iphone |

{style="table-layout:auto"}

+++

### SQL を使用したトークンの抽出 {#tokenization}

次の手順は、トークン化（テキスト分割）です。 トークン化とは、テキストを取り出し、個々の用語に分割するプロセスです。 通常、これは文を単語に分割することを伴います。 この例では、次のような SQL 関数を使用してトークンを抽出することで、文字列をバイグラム（および上位 n グラム）に分類します。 `regexp_extract_all`. 有効なトークン化を行うには、重複するバイグラムを生成する必要があります。

SQL はさらに改善され、 `regexp_extract_all`. `regexp_extract_all(lower(replace(ProductName, ' ', '')), '.{2}', 0) AS tokens:` クエリのこの部分では、変更された `ProductName` 前の手順で作成した値。 使用するのは `regexp_extract_all()` 関数を使用して、1 ～ 2 文字の重複しない部分文字列を、変更後の文字列と小文字からすべて抽出します。 `ProductName` 値。 The `.{2}` 正規表現パターンは、長さが 2 文字の部分文字列に一致します。 The `regexp_extract_all(..., '.{2}', 0)` 次に、関数の一部は、入力テキストから一致するすべてのサブ文字列を抽出します。

```SQL
SELECT DISTINCT(ProductName) AS featurevector1_distinct, lower(replace(ProductName, ' ', '')) AS featurevector1_transform, 
regexp_extract_all(lower(replace(ProductName, ' ', '')) , '.{2}', 0) AS tokens
FROM featurevector1;
```

結果を次の表に示します。

+++選択して展開します

|   | featurevector1_distinct | featurevector1_transform | トークン |
|---|--------------------------|--------------|------------------------|
| 1 | iPad Mini | ipadmini | {&quot;ip&quot;,&quot;ad&quot;,&quot;mi&quot;,&quot;ni&quot;} |
| 2 | iPad | iPad | {&quot;ip&quot;,&quot;ad&quot;} |
| 3 | iWatch | iWatch | {&quot;iw&quot;,&quot;at&quot;, &quot;ch&quot;} |
| 4 | iPhone | iPhone | {&quot;ip&quot;,&quot;ho&quot;,&quot;ne&quot;} |

{style="table-layout:auto"}

+++

精度をさらに向上させるには、SQL を使用して重なり合うトークンを作成する必要があります。 例えば、上記の「iPad」文字列に「pa」トークンがありません。 これを修正するには、先読み演算子をシフトします ( `substring`) を 1 つの手順で生成し、bi-gram を生成します。

前の手順と同様、 `regexp_extract_all(lower(replace(substring(ProductName, 2), ' ', '')), '.{2}', 0):` は、変更された製品名から 2 文字のシーケンスを抽出しますが、2 番目の文字から始まる場合は、 `substring` メソッドを使用して重複するトークンを作成します。 次に、3～7 行目 (`array_union(...) AS tokens`)、 `array_union()` 関数は、2 つの正規表現抽出で得られた 2 文字配列の配列を組み合わせます。 これにより、結果に重複しないシーケンスと重複しないシーケンスの両方で一意のトークンが含まれるようになります。

```SQL {line-numbers="true"}
SELECT DISTINCT(ProductName) AS featurevector1_distinct, 
       lower(replace(ProductName, ' ', '')) AS featurevector1_transform, 
       array_union(
           regexp_extract_all(lower(replace(ProductName, ' ', '')), '.{2}', 0),
           regexp_extract_all(lower(replace(substring(ProductName, 2), ' ', '')), '.{2}', 0)
       ) AS tokens
FROM featurevector1;
```

結果を次の表に示します。

+++選択して展開します

|   | featurevector1_distinct | featurevector1_transform | トークン |
|---|--------------------------|--------------|------------------------|
| 1 | iPad Mini | ipadmini | {&quot;ip&quot;,&quot;ad&quot;,&quot;mi&quot;,&quot;ni&quot;,&quot;pa&quot;,&quot;dm&quot;,&quot;in&quot;} |
| 2 | iPad | iPad | {&quot;ip&quot;,&quot;ad&quot;,&quot;pa&quot;} |
| 3 | iWatch | iWatch | {&quot;iw&quot;,&quot;at&quot;,&quot;ch&quot;,&quot;wa&quot;,&quot;tc&quot;} |
| 4 | iPhone | iPhone | {&quot;ip&quot;,&quot;ho&quot;,&quot;ne&quot;,&quot;ph&quot;,&quot;on&quot;} |

{style="table-layout:auto"}

+++

ただし、 `substring` 問題の解決策には制限があるので、 トリグラム（3 文字）に基づくテキストからトークンを作成する場合は、2 文字を使用する必要があります `substrings` 必要なシフトを得るために 2 回先を見る 10 グラムを作るには、9 つが必要です `substring` 式。 これによりコードが膨張し、維持できなくなります。 プレーンな正規表現の使用は不適切です。 新しいアプローチが必要です。

### 製品名の長さを調整 {#length-adjustment}

SQl は、シーケンス関数と長さ関数を使用して改善できます。 次の例では、 `sequence(1, length(lower(replace(ProductName, ' ', ''))) - 3)` は、1 から変更した製品名から 3 までの一連の番号を生成します。 たとえば、変更した製品名が文字長が 8 の「ipadmini」の場合、1 ～ 5(8 ～ 3) の数値が生成されます。

以下の文は、一意の製品名を抽出し、各名前を 4 文字の長さの文字（トークン）のシーケンス（スペースを除く）に分類し、2 列として表示します。 1 つの列には一意の製品名が、もう 1 つの列には生成されたトークンが表示されます。

```SQL
SELECT
   DISTINCT(ProductName) AS featurevector1_distinct,
  transform(
    sequence(1, length(lower(replace(ProductName, ' ', ''))) - 3),
    i -> substring(lower(replace(ProductName, ' ', '')), i, 4)
  ) AS tokens
FROM
  featurevector1;
```

結果を次の表に示します。

+++選択して展開します

|   | featurevector1_distinct | トークン |
|---|--------------------------|------------------------|
| 1 | iPad Mini | {&quot;ipad&quot;,&quot;padm&quot;,&quot;admi&quot;,&quot;dmin&quot;,&quot;mini&quot;} |
| 2 | iPad | {&quot;ipad&quot;} |
| 3 | iWatch | {&quot;iwat&quot;,&quot;watc&quot;,&quot;atch&quot;} |
| 4 | iPhone | {&quot;ipho&quot;,&quot;phone&quot;,&quot;hone&quot;} |

{style="table-layout:auto"}

+++

### トークンの長さの設定を確認 {#ensure-set-token-length}

生成されたシーケンスの長さを確実に指定するために、ステートメントに条件を追加できます。 次の SQL 文は、 `transform` 関数はより複雑です。 この文は、 `filter` 内部で機能する `transform` 生成されるシーケンスの長さが 6 文字になるようにします。 これらの位置に NULL 値を割り当てることで、これが不可能な場合に対処します。

```SQL
SELECT
  DISTINCT(ProductName) AS featurevector1_distinct,
  transform(
    filter(
      sequence(1, length(lower(replace(ProductName, ' ', ''))) - 5),
      i -> i + 5 <= length(lower(replace(ProductName, ' ', '')))
    ),
    i -> CASE WHEN length(substring(lower(replace(ProductName, ' ', '')), i, 6)) = 6
               THEN substring(lower(replace(ProductName, ' ', '')), i, 6)
               ELSE NULL
          END
  ) AS tokens
FROM
  featurevector1;
```

結果を次の表に示します。

+++選択して展開します

|   | featurevector1_distinct | トークン |
|---|--------------------------|------------------------|
| 1 | iPad Mini | {&quot;ipadmi&quot;,&quot;padmin&quot;,&quot;admini&quot;} |
| 2 | iPad | {null} |
| 3 | iWatch | {&quot;iwatch&quot;} |
| 4 | iPhone | {&quot;iphone&quot;} |

{style="table-layout:auto"}

+++

## Data Distillerの高次関数を使用したソリューションの調査 {#higher-order-function-solutions}

上位関数は、Data Distillerの構文と同様の「プログラミング」を実装できる強力な構成要素です。 これらを使用して、配列内の複数の値に対して関数を繰り返し実行できます。

Data Distillerのコンテキストでは、上位の関数は、n グラムを作成し、一連の文字を繰り返すのに最適です。

The `reduce` 関数、特に `transform`を使用すると、様々な分析および計画プロセスでピボット可能な、累積値または集計を導き出す方法が提供されます。

例えば、以下の SQl 文では、 `reduce()` 関数は、カスタム集計を使用して配列内の要素を集計します。 これは for ループをシミュレートし、 **すべての整数の累積合計を作成します** 1 から 5 まで `1, 1+2, 1+2+3, 1+2+3+4, 1+2+3+4`.

```SQL {line-numbers="true"}
SELECT transform(
    sequence(1, 5), 
    x -> reduce(
        sequence(1, x),  
        0,  -- Initial accumulator value
        (acc, y) -> acc + y  -- Higher-order function to add numbers
    )
) AS sum_result;
```

次に、SQL 文の分析を示します。

- 行 1: `transform` 関数を適用します。 `x -> reduce` を設定します。
- 行 2: `sequence(1, 5)` は、1 ～ 5 の数のシーケンスを生成します。
- 行 3: `x -> reduce(sequence(1, x), 0, (acc, y) -> acc + y)` は、シーケンス内の各要素 x に対して、1 ～ 5 の削減操作を実行します。
   - The `reduce` 関数は、初期アキュムレータ値 0 を取ります。この値は、1 から現在の値の順です。 `x`、および上位関数 `(acc, y) -> acc + y` をクリックして数字を追加します。
   - 上位関数 `acc + y` は、現在の値を追加することで合計を累積します `y` 蓄圧器に `acc`.
- 行 8: `AS sum_result` 結果の列の名前を sum_result に変更します。

要約すると、この高次関数は 2 つのパラメータ (`acc` および `y`) およびで、実行する操作を定義します（この場合は、を追加します）。 `y` 蓄圧器に `acc`. この高次関数は、縮小処理中に、シーケンス内の各要素に対して実行される。

この文の出力は、単一の列 (`sum_result`) には、1 ～ 5 の数値の累積合計が含まれます。

### 上位関数の値 {#value-of-higher-order-functions}

この節では、Data Distillerの上位関数の値をより深く理解し、n-gram をより効率的に作成するために、3-gram SQL 文の縮小版を分析します。

以下の文は、 `ProductName` 列内の `featurevector1` 表。 生成されたシーケンスから得られた位置を使用して、テーブル内の修正された製品名から派生した 3 文字の部分文字列のセットを生成します。

```SQL {line-numbers="true"}
SELECT
  transform(
    sequence(1, length(lower(replace(ProductName, ' ', ''))) - 2),
    i -> substring(lower(replace(ProductName, ' ', '')), i, 3)
  ) 
FROM
  featurevector1
```

次に、SQL 文の分析を示します。

- 行 2: `transform` シーケンス内の各整数に高次関数を適用します。
- 行 3: `sequence(1, length(lower(replace(ProductName, ' ', ''))) - 2)` から整数のシーケンスを生成します。 `1` を変更して、変更された製品名から 2 を引いた長さに設定します。
   - `length(lower(replace(ProductName, ' ', '')))` の長さを計算します。 `ProductName` を小文字にして、スペースを削除した後。
   - `- 2` 長さから 2 を引いて、3 文字の部分文字列の有効な開始位置を生成します。 2 を引くと、3 文字の部分文字列を抽出するのに十分な文字が各開始位置の後に残ります。 ここで指定したサブ文字列関数は、ルックアヘッド演算子のように機能します。
- 行 4: `i -> substring(lower(replace(ProductName, ' ', '')), i, 3)` は、各整数に対して動作する高次関数です。 `i` 生成されたシーケンス内。
   - The `substring(...)` 関数は、 `ProductName` 列。
   - 部分文字列を抽出する前に、 `lower(replace(ProductName, ' ', ''))` を `ProductName` を小文字にし、スペースを削除して一貫性を保ちます。

出力は、シーケンスで指定された位置に基づいて、変更された製品名から抽出された、3 文字の長さのサブ文字列のリストです。

## 結果のフィルター {#filter-results}

The `filter` 関数、後続 [データ変換](#data-transformation)を使用すると、テキストデータから関連情報をより絞り込んで正確に抽出できます。 これにより、インサイトを導き出し、データの品質を向上させ、より優れた意思決定プロセスを容易にすることができます。

The `filter` 次の SQL 文の関数は、後続の変換関数を使用して、文字列内の位置のシーケンスを調整および制限します。

```SQL
SELECT
  transform(
    filter(
      sequence(1, length(lower(replace(ProductName, ' ', ''))) - 6),
      i -> i + 6 <= length(lower(replace(ProductName, ' ', '')))
    ),
    i -> CASE WHEN length(substring(lower(replace(ProductName, ' ', '')), i, 7)) = 7
               THEN substring(lower(replace(ProductName, ' ', '')), i, 7)
               ELSE NULL
          END
  )
FROM
  featurevector1;
```

The `filter` 関数は、変更された `ProductName` とは、特定の長さの部分文字列を抽出します。 7 文字のサブ文字列の抽出を許可する開始位置のみが許可されます。

条件 `i -> i + 6 <= length(lower(replace(ProductName, ' ', '')))` 開始位置を確保する `i` プラス `6` （目的の 7 文字の部分文字列の長さから 1 を引いた長さ）が、変更された `ProductName`.

The `CASE` ステートメントは、長さに基づいてサブ文字列を条件付きで含めたり除外したりする場合に使用します。 7 文字の部分文字列のみが含まれ、その他は NULL に置き換えられます。 これらの部分文字列は、 `transform` 関数を使用して、 `ProductName` 列の `featurevector1` 表。

>[!TIP]
>
>以下を使用すると、 [パラメータ化テンプレート](../ui/parameterized-queries.md) クエリ内でロジックを再利用および抽象化する機能。 例えば、汎用ユーティリティ関数（上に示す文字列のトークン化用の関数など）を作成する場合、Data Distillerパラメータ化テンプレートを使用して、文字数をパラメータにすることができます。

## 2 つのフィーチャベクトルをまたいで一意の要素のクロス結合を計算します。 {#cross-join-unique-elements}

データの特定の変換に基づいて 2 つのデータセット間の違いや不一致を識別することは、データの正確性を維持し、データの品質を向上させ、データセット間の一貫性を確保するための一般的なプロセスです。

以下のこの SQL 文は、に存在する一意の製品名を抽出します。 `featurevector2` ただし、次に含まれない： `featurevector1` 変換を適用した後。

```SQL
SELECT lower(replace(ProductName, ' ', '')) FROM featurevector2
EXCEPT
SELECT lower(replace(ProductName, ' ', '')) FROM featurevector1;
```

>[!TIP]
>
>に加えて `EXCEPT`を使用する場合、 `UNION` および `INTERSECT` 使用例に応じて異なります。 また、 `ALL` または `DISTINCT` すべての値を含めて、指定した列の一意の値のみを返すという違いを確認する句を含める必要があります。

結果を次の表に示します。

+++選択して展開します

|   | lower(replace(ProductName, &#39; &#39;, &quot;)) |
|---|---------------------------------------|
| 1 | macbookpro |

{style="table-layout:auto"}

+++

次に、2 つのフィーチャベクトルの要素を結合して、比較する要素のペアを作成します。 このプロセスの最初の手順は、トークン化ベクトルを作成することです。

トークン化ベクトルは、各単語、フレーズ、または意味の単位（トークン）が数値形式に変換される、テキストデータの構造化表現です。 この変換により、自然言語処理アルゴリズムがテキスト情報を理解し、分析できるようになります。

以下の SQl は、トークン化ベクトルを作成します。

```SQL
CREATE TABLE featurevector1tokenized AS SELECT
  DISTINCT(ProductName) AS featurevector1_distinct,
  transform(
    filter(
      sequence(1, length(lower(replace(ProductName, ' ', ''))) - 1),
      i -> i + 1 <= length(lower(replace(ProductName, ' ', '')))
    ),
    i -> CASE WHEN length(substring(lower(replace(ProductName, ' ', '')), i, 2)) = 2
               THEN substring(lower(replace(ProductName, ' ', '')), i, 2)
               ELSE NULL
          END
  ) AS tokens
FROM
  (SELECT lower(replace(ProductName, ' ', '')) AS ProductName FROM featurevector1);
SELECT * FROM featurevector1tokenized;
```

>[!NOTE]
>
>を使用している場合、 [!DNL DbVisualizer]テーブルを作成または削除した後、データベース接続を更新して、テーブルのメタデータキャッシュが更新されるようにします。 Data Distillerは、メタデータの更新をプッシュアウトしません。

結果を次の表に示します。

+++選択して展開します

|   | featurevector1_distinct | トークン |
|---|--------------------------|------------------------|
| 1 | ipadmini | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;,&quot;dm&quot;,&quot;mi&quot;,&quot;in&quot;,&quot;ni&quot;} |
| 2 | ipad | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;} |
| 3 | iwatch | {&quot;iw&quot;,&quot;wa&quot;,&quot;at&quot;,&quot;tc&quot;,&quot;ch&quot;} |
| 4 | iphone | {&quot;ip&quot;,&quot;ph&quot;,&quot;ho&quot;,&quot;on&quot;,&quot;ne&quot;} |

{style="table-layout:auto"}

+++

次に、次の手順を繰り返します： `featurevector2`:

```SQL
CREATE TABLE featurevector2tokenized AS 
SELECT
  DISTINCT(ProductName) AS featurevector2_distinct,
  transform(
    filter(
      sequence(1, length(lower(replace(ProductName, ' ', ''))) - 1),
      i -> i + 1 <= length(lower(replace(ProductName, ' ', '')))
    ),
    i -> CASE WHEN length(substring(lower(replace(ProductName, ' ', '')), i, 2)) = 2
               THEN substring(lower(replace(ProductName, ' ', '')), i, 2)
               ELSE NULL
          END
  ) AS tokens
FROM
(SELECT lower(replace(ProductName, ' ', '')) AS ProductName FROM featurevector2
);
SELECT * FROM featurevector2tokenized;
```

結果を次の表に示します。

+++選択して展開します

|   | featurevector2_distinct | トークン |
|---|--------------------------|------------------------|
| 1 | ipadmini | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;} |
| 2 | macbookpro | {&quot;ma&quot;,&quot;ac&quot;,&quot;cb&quot;,&quot;bo&quot;,&quot;oo&quot;,&quot;ok&quot;,&quot;kp&quot;,&quot;pr&quot;,&quot;ro&quot;} |
| 3 | iphone | {&quot;ip&quot;,&quot;ph&quot;,&quot;ho&quot;,&quot;on&quot;,&quot;ne&quot;} |

{style="table-layout:auto"}

+++

両方のトークン化ベクトルが完了したら、クロス結合を作成できます。 これは、次の SQL で見られます。

```SQL {line-numbers="true"}
SELECT
    A.featurevector1_distinct AS SetA_ProductNames,
    B.featurevector2_distinct AS SetB_ProductNames,
    A.tokens AS SetA_tokens1,
    B.tokens AS SetB_tokens2
FROM
    featurevector1tokenized A
CROSS JOIN
    featurevector2tokenized B;
```

次に、クロス結合の作成に使用する SQl の概要を示します。

- 行 2: `A.featurevector1_distinct AS SetA_ProductNames` を選択します。 `featurevector1_distinct` テーブルの列 `A` そして別名を割り当てる `SetA_ProductNames`. SQL のこのセクションでは、最初のデータセットからの個別の製品名のリストが生成されます。
- 行 4: `A.tokens AS SetA_tokens1` を選択します。 `tokens` テーブルまたはサブクエリの列 `A` そして別名を割り当てる `SetA_tokens1`. SQL のこのセクションでは、最初のデータセットの製品名に関連付けられたトークン化された値のリストが表示されます。
- 8 行目： `CROSS JOIN` この操作は、2 つのデータセットの行の組み合わせをすべて組み合わせます。 つまり、各製品名と、その最初のテーブル (`A`) に、2 番目のテーブル (`B`) をクリックします。 これにより、2 つのデータセットの直交積が生成され、出力の各行は、製品名と、両方のデータセットの関連トークンの組み合わせを表します。

結果を次の表に示します。

+++選択して展開します

| * | SetA_ProductNames | SetB_ProductNames | SetA_tokens 1 | SetB_tokens 2 |
|---|---------------------|-------------------|---|---|
| 1 | ipadmini | ipad | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;,&quot;dm&quot;,&quot;mi&quot;,&quot;in&quot;,&quot;ni&quot;} | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;} |
| 2 | ipadmini | macbookpro | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;,&quot;dm&quot;,&quot;mi&quot;,&quot;in&quot;,&quot;ni&quot;} | {&quot;ma&quot;,&quot;ac&quot;,&quot;cb&quot;,&quot;bo&quot;,&quot;oo&quot;,&quot;ok&quot;,&quot;kp&quot;,&quot;pr&quot;,&quot;ro&quot;} |
| 3 | ipadmini | iphone | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;,&quot;dm&quot;,&quot;mi&quot;,&quot;in&quot;,&quot;ni&quot;} | {&quot;ip&quot;,&quot;ph&quot;,&quot;ho&quot;,&quot;on&quot;,&quot;ne&quot;} |
| 4 | ipad | ipad | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;} | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;} |
| 5 | ipad | macbookpro | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;} | {&quot;ma&quot;,&quot;ac&quot;,&quot;cb&quot;,&quot;bo&quot;,&quot;oo&quot;,&quot;ok&quot;,&quot;kp&quot;,&quot;pr&quot;,&quot;ro&quot;} |
| 6 | ipad | iphone | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;} | {&quot;ip&quot;,&quot;ph&quot;,&quot;ho&quot;,&quot;on&quot;,&quot;ne&quot;} |
| 7 | iwatch | ipad | {&quot;iw&quot;,&quot;wa&quot;,&quot;at&quot;,&quot;tc&quot;,&quot;ch&quot;} | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;} |
| 8 | iwatch | macbookpro | {&quot;iw&quot;,&quot;wa&quot;,&quot;at&quot;,&quot;tc&quot;,&quot;ch&quot;} | {&quot;ma&quot;,&quot;ac&quot;,&quot;cb&quot;,&quot;bo&quot;,&quot;oo&quot;,&quot;ok&quot;,&quot;kp&quot;,&quot;pr&quot;,&quot;ro&quot;} |
| 9 | iwatch | iphone | {&quot;iw&quot;,&quot;wa&quot;,&quot;at&quot;,&quot;tc&quot;,&quot;ch&quot;} | {&quot;ip&quot;,&quot;ph&quot;,&quot;ho&quot;,&quot;on&quot;,&quot;ne&quot;} |
| 10 | iphone | ipad | {&quot;ip&quot;,&quot;ph&quot;,&quot;ho&quot;,&quot;on&quot;,&quot;ne&quot;} | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;} |
| 11 | iphone | macbookpro | {&quot;ip&quot;,&quot;ph&quot;,&quot;ho&quot;,&quot;on&quot;,&quot;ne&quot;} | {&quot;ma&quot;,&quot;ac&quot;,&quot;cb&quot;,&quot;bo&quot;,&quot;oo&quot;,&quot;ok&quot;,&quot;kp&quot;,&quot;pr&quot;,&quot;ro&quot;} |
| 12 | iphone | iphone | {&quot;ip&quot;,&quot;ph&quot;,&quot;ho&quot;,&quot;on&quot;,&quot;ne&quot;} | {&quot;ip&quot;,&quot;ph&quot;,&quot;ho&quot;,&quot;on&quot;,&quot;ne&quot;} |

{style="table-layout:auto"}

+++

## Jaccard 類似性測定を計算 {#compute-the-jaccard-similarity-measure}

次に、Jaccard の類似性係数を使用して、2 つの製品名のトークン化された表現を比較し、類似性分析を実行します。 次の SQL スクリプトの出力は、両方のセットの製品名、トークン化された表現、共通トークンと合計個別トークンの数、データセットの各ペアに対する計算済みの Jaccard 類似性係数を示します。


```SQL {line-numbers="true"}
SELECT 
    SetA_ProductNames, 
    SetB_ProductNames, 
    SetA_tokens1,
    SetB_tokens2,
    size(array_intersect(SetA_tokens1, SetB_tokens2)) AS token_intersect_count,
    size(array_union(SetA_tokens1, SetB_tokens2)) AS token_union_count,
    ROUND(
        CAST(size(array_intersect(SetA_tokens1, SetB_tokens2)) AS DOUBLE) /    size(array_union(SetA_tokens1, SetB_tokens2)), 2) AS jaccard_similarity
FROM
    (SELECT
        A.featurevector1_distinct AS SetA_ProductNames,
        B.featurevector2_distinct AS SetB_ProductNames,
        A.tokens AS SetA_tokens1,
        B.tokens AS SetB_tokens2
    FROM
        featurevector1tokenized A
    CROSS JOIN
        featurevector2tokenized B
    );
```

Jaccard 類似性係数の計算に使用する SQL の概要を次に示します。

- 行 6: `size(array_intersect(SetA_tokens1, SetB_tokens2)) AS token_intersect_count` 両方に共通するトークンの数を計算します `SetA_tokens1` および `SetB_tokens2`. この計算は、2 つのトークンの配列の積集合のサイズを計算することで実現されます。
- 行 7: `size(array_union(SetA_tokens1, SetB_tokens2)) AS token_union_count` 両方のトークンのユニークトークンの総数を計算します `SetA_tokens1` および `SetB_tokens2`. この行は、2 つのトークンの配列の和集合のサイズを計算します。
- 行 8～10: `ROUND(CAST(size(array_intersect(SetA_tokens1, SetB_tokens2)) AS DOUBLE) / size(array_union(SetA_tokens1, SetB_tokens2)), 2) AS jaccard_similarity` トークンセット間の Jaccard 類似性を計算します。 これらの線は、トークンの積集合のサイズをトークンの和集合のサイズで割り、結果を小数点以下 2 桁に丸めます。 結果は 0 ～ 1 の値になります。1 つは完全な類似性を示します。

結果を次の表に示します。

+++選択して展開します

| * | SetA_ProductNames | SetB_ProductNames | SetA_tokens 1 | SetB_tokens 2 | token_intersect_count | token_intersect_count | Jaccard 類似性 |
|---|---------------------|-------------------|---------------------------------------|-------------------------------------------------|----|----|----|
| 1 | ipadmini | ipad | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;,&quot;dm&quot;,&quot;mi&quot;,&quot;in&quot;,&quot;ni&quot;} | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;} | 3 | 7 | 0.43 |
| 2 | ipadmini | macbookpro | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;,&quot;dm&quot;,&quot;mi&quot;,&quot;in&quot;,&quot;ni&quot;} | {&quot;ma&quot;,&quot;ac&quot;,&quot;cb&quot;,&quot;bo&quot;,&quot;oo&quot;,&quot;ok&quot;,&quot;kp&quot;,&quot;pr&quot;,&quot;ro&quot;} | 0 | 16 | 0.0 |
| 3 | ipadmini | iphone | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;,&quot;dm&quot;,&quot;mi&quot;,&quot;in&quot;,&quot;ni&quot;} | {&quot;ip&quot;,&quot;ph&quot;,&quot;ho&quot;,&quot;on&quot;,&quot;ne&quot;} | 1 | 11 | 0.09 |
| 4 | ipad | ipad | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;} | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;} | 3 | 3 | 1.0 |
| 5 | ipad | macbookpro | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;} | {&quot;ma&quot;,&quot;ac&quot;,&quot;cb&quot;,&quot;bo&quot;,&quot;oo&quot;,&quot;ok&quot;,&quot;kp&quot;,&quot;pr&quot;,&quot;ro&quot;} | 0 | 12 | 0.0 |
| 6 | ipad | iphone | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;} | {&quot;ip&quot;,&quot;ph&quot;,&quot;ho&quot;,&quot;on&quot;,&quot;ne&quot;} | 1 | 7 | 0.14 |
| 7 | iwatch | ipad | {&quot;iw&quot;,&quot;wa&quot;,&quot;at&quot;,&quot;tc&quot;,&quot;ch&quot;} | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;} | 0 | 8 | 0.0 |
| 8 | iwatch | macbookpro | {&quot;iw&quot;,&quot;wa&quot;,&quot;at&quot;,&quot;tc&quot;,&quot;ch&quot;} | {&quot;ma&quot;,&quot;ac&quot;,&quot;cb&quot;,&quot;bo&quot;,&quot;oo&quot;,&quot;ok&quot;,&quot;kp&quot;,&quot;pr&quot;,&quot;ro&quot;} | 0 | 14 | 0.0 |
| 9 | iwatch | iphone | {&quot;iw&quot;,&quot;wa&quot;,&quot;at&quot;,&quot;tc&quot;,&quot;ch&quot;} | {&quot;ip&quot;,&quot;ph&quot;,&quot;ho&quot;,&quot;on&quot;,&quot;ne&quot;} | 0 | 10 | 0.0 |
| 10 | iphone | ipad | {&quot;ip&quot;,&quot;ph&quot;,&quot;ho&quot;,&quot;on&quot;,&quot;ne&quot;} | {&quot;ip&quot;,&quot;pa&quot;,&quot;ad&quot;} | 1 | 7 | 0.14 |
| 11 | iphone | macbookpro | {&quot;ip&quot;,&quot;ph&quot;,&quot;ho&quot;,&quot;on&quot;,&quot;ne&quot;} | {&quot;ma&quot;,&quot;ac&quot;,&quot;cb&quot;,&quot;bo&quot;,&quot;oo&quot;,&quot;ok&quot;,&quot;kp&quot;,&quot;pr&quot;,&quot;ro&quot;} | 0 | 14 | 0.0 |
| 12 | iphone | iphone | {&quot;ip&quot;,&quot;ph&quot;,&quot;ho&quot;,&quot;on&quot;,&quot;ne&quot;} | {&quot;ip&quot;,&quot;ph&quot;,&quot;ho&quot;,&quot;on&quot;,&quot;ne&quot;} | 5 | 5 | 1.0 |

{style="table-layout:auto"}

+++

## Jaccard 類似性しきい値に基づいて結果をフィルタリングします {#similarity-threshold-filter}

最後に、事前定義されたしきい値に基づいて結果をフィルタリングし、類似性の条件を満たすペアのみを選択します。 以下の SQL 文は、Jaccard 類似係数が 0.4 以上の製品をフィルタリングします。これにより、結果を絞り込み、相当程度の類似性を示すペアを作成します。

```SQL
SELECT 
    SetA_ProductNames, 
    SetB_ProductNames
FROM 
(SELECT 
    SetA_ProductNames, 
    SetB_ProductNames, 
    SetA_tokens1,
    SetB_tokens2,
    size(array_intersect(SetA_tokens1, SetB_tokens2)) AS token_intersect_count,
    size(array_union(SetA_tokens1, SetB_tokens2)) AS token_union_count,
    ROUND(
        CAST(size(array_intersect(SetA_tokens1, SetB_tokens2)) AS DOUBLE) / size(array_union(SetA_tokens1, SetB_tokens2)),
        2
    ) AS jaccard_similarity
FROM
    (SELECT
        A.featurevector1_distinct AS SetA_ProductNames,
        B.featurevector2_distinct AS SetB_ProductNames,
        A.tokens AS SetA_tokens1,
        B.tokens AS SetB_tokens2
    FROM
        featurevector1tokenized A
    CROSS JOIN
        featurevector2tokenized B
    )
)
WHERE jaccard_similarity>=0.4
```

このクエリの結果は、次に示すように、類似性結合の列を提供します。

+++選択して展開します

|   | SetA_ProductNames | SetA_ProductNames |
|---|--------------------------|------------------------|
| 1 | ipadmini | ipad |
| 2 | ipad | ipad |
| 3 | iphone | iphone |

{style="table-layout:auto"}

+++:

### 次の手順 {#next-steps}

このドキュメントを読むと、このロジックを使用して、意味のある関係や異なるデータセット間の重複を強調表示できます。 特性や属性に大きな類似点がある異なるデータセットから製品を識別する機能には、多数の実際のアプリケーションがあります。 このロジックは、次のようなシナリオで使用できます。

- 製品のマッチング：顧客に類似した製品をグループ化またはレコメンデーションします。
- データクレンジング：データの品質を向上させます。
- マーケットバスケット分析：顧客の行動、好み、潜在的なクロスセルの機会に関するインサイトを提供します。

まだ読んでいない場合は、 [AI/ML 機能パイプラインの概要](../data-distiller/ml-feature-pipelines/overview.md). この概要を使用して、Data Distillerとお客様の好みの機械学習が、Experience Platformデータを使用したマーケティングの使用例をサポートするカスタムデータモデルを構築する方法を学びます。
