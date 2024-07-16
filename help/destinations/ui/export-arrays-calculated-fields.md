---
title: （ベータ版）計算フィールドを使用したフラットスキーマファイルでの配列の書き出し
type: Tutorial
description: 計算フィールドを使用して、Real-Time CDPからクラウドストレージの宛先にフラットスキーマファイルの配列を書き出す方法を説明します。
badge: ベータ版
exl-id: ff13d8b7-6287-4315-ba71-094e2270d039
source-git-commit: 787aaef26fab5ca3acff8303f928efa299cafa93
workflow-type: tm+mt
source-wordcount: '1477'
ht-degree: 5%

---

# （ベータ版）計算フィールドを使用したフラットスキーマファイルでの配列の書き出し {#use-calculated-fields-to-export-arrays-in-flat-schema-files}

>[!CONTEXTUALHELP]
>id="platform_destinations_export_arrays_flat_files"
>title="（ベータ版）書き出し配列のサポート"
>abstract="**計算フィールドを追加**&#x200B;コントロールを使用して、int、文字列、またブーリアン値の単純な配列を Experience Platform から目的のクラウドストレージの宛先に書き出します。いくつかの制限が適用されます。広範な例とサポートされている関数については、ドキュメントを参照してください。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/export-arrays-calculated-fields.html?lang=ja#examples" text="例"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/export-arrays-calculated-fields.html?lang=ja#known-limitations" text="既知の制限事項"

>[!AVAILABILITY]
>
>* 計算フィールドを使用して配列を書き出す機能は、現在Betaにあります。 ドキュメントと機能は変更される場合があります。

フラットなスキーマファイルのReal-Time CDPから [ クラウドストレージの宛先 ](/help/destinations/catalog/cloud-storage/overview.md) に計算フィールドを使用して配列を書き出す方法を説明します。 このドキュメントを読んで、この機能で有効になるユースケースを理解します。

計算フィールドの概要と重要性に関する情報を説明します。 データ準備の計算フィールドの概要と、使用可能なすべての関数の詳細については、以下にリンクしたページを参照してください。

* [UI ガイドと概要](/help/data-prep/ui/mapping.md#calculated-fields)
* [Data Prep 関数](/help/data-prep/functions.md)

<!--

>[!IMPORTANT]
>
>Not all functions listed above are supported *when exporting fields to cloud storage destinations* using the calculated fields functionality. See the [supported functions section](#supported-functions) further below for more information.

-->

## Platform の配列およびその他のオブジェクトタイプ {#arrays-strings-other-objects}

Experience Platformでは、[XDM スキーマ ](/help/xdm/home.md) を使用して、様々なフィールドタイプを管理できます。 以前は、Experience Platform外の文字列など、単純なキーと値のペアのフィールドを目的の宛先に書き出すことができました。 以前に書き出し用にサポートされていたフィールドの例は、`personalEmail.address`:`johndoe@acme.org` です。

Experience Platformのその他のフィールドタイプには、配列フィールドが含まれます。 詳しくは、[Experience PlatformUI での配列フィールドの管理 ](/help/xdm/ui/fields/array.md) を参照してください。 以前にサポートされていたフィールドタイプに加えて、`organizations:[marketing, sales, engineering]` などの配列オブジェクトを書き出すことができるようになりました。 様々な関数を使用して配列の要素にアクセスする方法や、配列要素を文字列に結合する方法については、後述の [ 広範な例 ](#examples) を参照してください。

## 既知の制限事項 {#known-limitations}

この機能のベータ版リリースには、次の既知の制限事項があります。

* 階層スキーマを使用した JSON ファイルまたは Parquet ファイルへの書き出しは、現時点ではサポートされていません。 配列は、フラットスキーマの CSV、JSON、Parquet ファイルにのみ書き出すことができます。
* 現時点では、*単純な配列（またはプリミティブ値の配列）のみをクラウドストレージの宛先に書き出すことができます*。 つまり、文字列、整数またはブール値を含む配列オブジェクトを書き出すことができます。 マップまたはマップまたはオブジェクトの配列をエクスポートすることはできません。計算フィールドのモーダルウィンドウには、エクスポート可能な配列のみが表示されます。

## 前提条件 {#prerequisites}

[ 接続 ](/help/destinations/ui/connect-destination.md) を目的のクラウドストレージの宛先に対して行い、[ クラウドストレージの宛先のアクティベーション手順 ](/help/destinations/ui/activate-batch-profile-destinations.md) の手順を実行して、[ マッピング ](/help/destinations/ui/activate-batch-profile-destinations.md#mapping) の手順に進みます。

## 計算フィールドのエクスポート方法 {#how-to-export-calculated-fields}

クラウドストレージ宛先のアクティベーションワークフローのマッピング手順で、「**[!UICONTROL （Beta）計算フィールドを追加]**」を選択します。

![ バッチアクティベーションワークフローのマッピング手順でハイライト表示された計算フィールドを追加。](/help/destinations/assets/ui/export-arrays-calculated-fields/add-calculated-fields.png)

これにより、Experience Platformから属性を書き出すために使用できる属性を選択できるモーダルウィンドウが開きます。

>[!IMPORTANT]
>
>**[!UICONTROL フィールド]** ビューでは、XDM スキーマの一部のフィールドのみ使用できます。 文字列値と、文字列、整数、ブール値の配列を確認できます。 例えば、他の配列値が含まれているので、`segmentMembership` 配列は表示されません。

![ 関数がまだ選択されていない計算フィールド機能のモーダルウィンドウ。](/help/destinations/assets/ui/export-arrays-calculated-fields/add-calculated-fields-2.png)

例えば、以下に示すように、「`loyaltyID`」フィールドで `join` 関数を使用して、ロイヤルティ ID の配列を、CSV ファイル内のアンダースコアで連結された文字列として書き出します。 [ この例およびその他の例の詳細は以下 ](#join-function-export-arrays) をご覧ください。

![ 結合関数が選択された計算フィールド機能のモーダルウィンドウ。](/help/destinations/assets/ui/export-arrays-calculated-fields/add-calculated-fields-3.png)

「**[!UICONTROL 保存]**」を選択して計算フィールドを保持し、マッピングステップに戻ります。

![ 結合関数が選択され、「保存」コントロールがハイライト表示された計算フィールド機能のモーダルウィンドウ。](/help/destinations/assets/ui/export-arrays-calculated-fields/save-calculated-field.png)

ワークフローのマッピング手順に戻り、書き出されたファイルのこのフィールドに必要な列ヘッダーの値を **[!UICONTROL ターゲットフィールド]** に入力します。

![ ターゲットフィールドがハイライト表示されたマッピングステップ ](/help/destinations/assets/ui/export-arrays-calculated-fields/fill-in-target-field.png)

![ ターゲットフィールド 2 を選択 ](/help/destinations/assets/ui/export-arrays-calculated-fields/target-field-filled-in.png)

準備ができたら、「**[!UICONTROL 次へ]** を選択して、アクティベーションワークフローの次のステップに進みます。

![ ターゲットフィールドがハイライト表示され、ターゲット値が入力されたマッピングステップ ](/help/destinations/assets/ui/export-arrays-calculated-fields/select-next-to-proceed.png)

## サポートされる関数 {#supported-functions}

ファイルベースの宛先に対してデータをアクティブ化する場合、ドキュメントに記載されているすべての [ データ準備関数 ](/help/data-prep/functions.md) がサポートされます。

ただし、広範なユースケースの説明とサンプル出力情報は、現在、計算フィールドのベータ版リリースおよび宛先の配列サポートで、次の関数に対してのみ提供されています。

* `join`
* `coalesce`
* `size_of`
* `iif`
* `index-based array access`
* `add_to_array`
* `to_array`
* `first`
* `last`
* `sha256`
* `md5`

## 配列の書き出しに使用する関数の例 {#examples}

上記の関数の一部については、以下の節の例と詳細情報を参照してください。 リストに表示されたその他の関数については、[ データ準備セクションの汎用関数のドキュメント ](/help/data-prep/functions.md) を参照してください。

### 配列を書き出す `join` 関数 {#join-function-export-arrays}

`join` 関数を使用すると、`_` や `|` などの目的の区切り文字を使用して、配列の要素を文字列に連結できます。

例えば、`join('_',loyalty.loyaltyID)` 構文を使用して、マッピングのスクリーンショットに示すように、以下の XDM フィールドを組み合わせることができます。

* `"organizations": ["Marketing","Sales,"Finance"]` 配列
* `person.name.firstName` 文字列
* `person.name.lastName` 文字列
* `personalEmail.address` 文字列

![ 結合関数を含むマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-join-function.png)

この場合、出力ファイルは次のようになります。 `_` 文字を使用して、配列の 3 つの要素が 1 つの文字列に連結されていることに注意してください。

```
`First_Name,Last_Name,Personal_Email,Organization
John,Doe,johndoe@acme.org, "Marketing_Sales_Finance"
```

### 配列を書き出す `iif` 関数 {#iif-function-export-arrays}

特定の条件下で配列の要素を書き出すには、`iif` 関数を使用します。 例えば、上記の `organizations` 配列オブジェクトを続けて、`iif(organizations[0].equals("Marketing"), "isMarketing", "isNotMarketing")` のような単純な条件関数を記述できます。

![iif 関数を含むマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-iif-function.png)

この場合、出力ファイルは次のようになります。 この場合、配列の最初の要素はマーケティングなので、その人物はマーケティング部門のメンバーです。

```
`First_Name,Last_Name, Personal_Email, Is_Member_Of_Marketing_Dept
John,Doe, johndoe@acme.org, "isMarketing"
```

### 配列を書き出す `add_to_array` 関数 {#add-to-array-function-export-arrays}

書き出された配列に要素を追加するには、`add_to_array` 関数を使用します。 この関数は、上記の `join` 関数と組み合わせることができます。

上記の `organizations` 配列オブジェクトを続けて、`source: join('_', add_to_array(organizations,"2023"))` のような関数を記述して、2023 年にユーザーが属する組織を返すことができます。

![add_to_array 関数を含むマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-add-to-array-function.png)

この場合、出力ファイルは次のようになります。 配列の 3 つの要素が、`_` 文字を使用して 1 つの文字列に連結され、文字列の末尾にも 2023 が追加されていることに注意してください。

```
`First_Name,Last_Name,Personal_Email,Organization_Member_2023
John,Doe, johndoe@acme.org,"Marketing_Sales_Finance_2023"
```

### 配列を書き出す `coalesce` 関数 {#coalesce-function-export-arrays}

`coalesce` 関数を使用して、配列の最初の null 以外の要素にアクセスし、文字列に書き出します。

例えば、`coalesce(subscriptions.hasPromotion)` 構文を使用して、配列の値の最初の `true` を返すことで、マッピングスクリーンショットに示されているように、以下の XDM フィールド `false` 組み合わせることができます。

* `"subscriptions.hasPromotion": [null, true, null, false, true]` 配列
* `person.name.firstName` 文字列
* `person.name.lastName` 文字列
* `personalEmail.address` 文字列

![coalesce 関数を含むマッピングの例 ](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-coalesce-function.png)

この場合、出力ファイルは次のようになります。 配列の最初の null 以外の `true` 値がファイルに書き出されます。

```
First_Name,Last_Name,hasPromotion
John,Doe,true
```

### 配列を書き出す `size_of` 関数 {#sizeof-function-export-arrays}

`size_of` 関数を使用して、配列内に存在する要素の数を示します。 例えば、複数のタイムスタンプを持つ `purchaseTime` 配列オブジェクトがある場合、`size_of` 関数を使用して、ある人物が個別に購入した回数を示すことができます。

例えば、マッピングのスクリーンショットに示すように、以下の XDM フィールドを組み合わせることができます。

* お客様による 5 つの個別の購入時間を示す `"purchaseTime": ["1538097126","1569633126,"1601255526","1632791526","1664327526"]` 配列
* `personalEmail.address` 文字列

![size_of 関数を含むマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-size-of-function.png)

この場合、出力ファイルは次のようになります。 2 番目の列が、顧客による個別の購入の数に対応する、配列の要素の数を示していることに注意してください。

```
`Personal_Email,Times_Purchased
johndoe@acme.org,"5"
```

### インデックス・ベースのアレイ・アクセス {#index-based-array-access}

配列のインデックスにアクセスして、配列から 1 つの項目を書き出すことができます。 例えば、`size_of` 関数の前述の例と同様に、顧客が特定の製品を初めて購入したときにのみアクセスして書き出しを行う場合は、`purchaseTime[0]` を使用してタイムスタンプの最初の要素を書き出 `purchaseTime[1]`、タイムスタンプの 2 番目の要素を書き出 `purchaseTime[2]`、タイムスタンプの 3 番目の要素を書き出すことができます。

![ 配列の要素にアクセスする方法を示すマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-index.png)

この場合、出力ファイルは次のようになります。顧客が初めて購入したときに書き出されます。

```
`Personal_Email,First_Purchase
johndoe@acme.org,"1538097126"
```

### 配列を書き出す `first` 関数と `last` 関数 {#first-and-last-functions-export-arrays}

`first` 関数と `last` 関数を使用して、配列の最初または最後の要素を書き出します。 例えば、前の例で使用した複数のタイムスタンプを含む `purchaseTime` 配列オブジェクトで続けて、これらを使用して、人物による最初または最後の購入時間を書き出す関数を使用できます。

![first 関数と last 関数を含むマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-first-last-functions.png)

この場合、出力ファイルは次のようになります。顧客が購入を行った最初と最後の時間が書き出されます。

```
`Personal_Email,First_Purchase, Last_Purchase
johndoe@acme.org,"1538097126","1664327526"
```

### ハッシュ関数 {#hashing-functions}

配列または要素を配列から書き出すための関数に加えて、ハッシュ関数を使用して、書き出されたファイルの属性をハッシュ化できます。 例えば、属性に個人を特定できる情報がある場合、それらのフィールドを書き出す際にハッシュ化できます。

文字列値を直接ハッシュ化できます（例：`md5(personalEmail.address)`）。 必要に応じて、配列フィールドの要素が次のような文字列であると仮定して、配列フィールドの個々の要素をハッシュ化することもできます。`md5(purchaseTime[0])`

サポートされているハッシュ関数は次のとおりです。

| 関数 | サンプル式 |
|---------|----------|
| `sha1` | `sha1(organizations[0])` |
| `sha256` | `sha256(organizations[0])` |
| `sha512` | `sha512(organizations[0])` |
| `hash` | `hash("crc32", organizations[0], "UTF-8")` |
| `md5` | `md5(organizations[0], "UTF-8")` |
| `crc32` | `crc32(organizations[0])` |

{style="table-layout:auto"}