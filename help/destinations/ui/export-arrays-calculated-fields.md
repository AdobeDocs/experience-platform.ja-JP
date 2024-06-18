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
>* 計算フィールドを使用して配列を書き出す機能は、現在ベータ版です。 ドキュメントと機能は変更される場合があります。

フラットなスキーマファイルのReal-Time CDPから計算フィールドを使用して配列を書き出す方法を説明します。 [クラウドストレージの宛先](/help/destinations/catalog/cloud-storage/overview.md). このドキュメントを読んで、この機能で有効になるユースケースを理解します。

計算フィールドの概要と重要性に関する情報を説明します。 データ準備の計算フィールドの概要と、使用可能なすべての関数の詳細については、以下にリンクしたページを参照してください。

* [UI ガイドと概要](/help/data-prep/ui/mapping.md#calculated-fields)
* [Data Prep 関数](/help/data-prep/functions.md)

<!--

>[!IMPORTANT]
>
>Not all functions listed above are supported *when exporting fields to cloud storage destinations* using the calculated fields functionality. See the [supported functions section](#supported-functions) further below for more information.

-->

## Platform の配列およびその他のオブジェクトタイプ {#arrays-strings-other-objects}

Experience Platformでは、次を使用できます [XDM スキーマ](/help/xdm/home.md) 様々なフィールドタイプを管理できます。 以前は、Experience Platform外の文字列など、単純なキーと値のペアのフィールドを目的の宛先に書き出すことができました。 以前に書き出しでサポートされていたフィールドの例は次のとおりです `personalEmail.address`:`johndoe@acme.org`.

Experience Platformのその他のフィールドタイプには、配列フィールドが含まれます。 詳細を読む： [Experience PlatformUI での配列フィールドの管理](/help/xdm/ui/fields/array.md). 以前にサポートされていたフィールドタイプに加えて、次のような配列オブジェクトを書き出すことができるようになりました。 `organizations:[marketing, sales, engineering]`. 後述の [広範な例](#examples) 様々な関数を使用して配列の要素にアクセスする方法、配列要素を文字列に結合する方法など。

## 既知の制限事項 {#known-limitations}

この機能のベータ版リリースには、次の既知の制限事項があります。

* 階層スキーマを使用した JSON ファイルまたは Parquet ファイルへの書き出しは、現時点ではサポートされていません。 配列は、フラットスキーマの CSV、JSON、Parquet ファイルにのみ書き出すことができます。
* この時点で、 *単純な配列（またはプリミティブ値の配列）のみをクラウドストレージの宛先に書き出すことができます*. つまり、文字列、整数またはブール値を含む配列オブジェクトを書き出すことができます。 マップまたはマップまたはオブジェクトの配列をエクスポートすることはできません。計算フィールドのモーダルウィンドウには、エクスポート可能な配列のみが表示されます。

## 前提条件 {#prerequisites}

[接続](/help/destinations/ui/connect-destination.md) 目的のクラウドストレージの宛先に移動するには、を実行します [クラウドストレージの宛先のアクティベーション手順](/help/destinations/ui/activate-batch-profile-destinations.md) に移動します。 [マッピング](/help/destinations/ui/activate-batch-profile-destinations.md#mapping) ステップ。

## 計算フィールドのエクスポート方法 {#how-to-export-calculated-fields}

クラウドストレージ宛先のアクティベーションワークフローのマッピング手順で、次の項目を選択します **[!UICONTROL （ベータ版）計算フィールドの追加]**.

![バッチアクティベーションワークフローのマッピング手順でハイライト表示された計算フィールドを追加します。](/help/destinations/assets/ui/export-arrays-calculated-fields/add-calculated-fields.png)

これにより、Experience Platformから属性を書き出すために使用できる属性を選択できるモーダルウィンドウが開きます。

>[!IMPORTANT]
>
>では、XDM スキーマの一部のフィールドのみ使用できます **[!UICONTROL フィールド]** 表示。 文字列値と、文字列、整数、ブール値の配列を確認できます。 例： `segmentMembership` 他の配列値が含まれているので、配列が表示されない。

![関数がまだ選択されていない計算フィールド機能のモーダルウィンドウ。](/help/destinations/assets/ui/export-arrays-calculated-fields/add-calculated-fields-2.png)

例えば、 `join` 関数 `loyaltyID` 以下に示すフィールドに、CSV ファイル内のアンダースコアで連結された文字列としてロイヤルティ ID の配列を書き出します。 表示 [この例およびその他の例については、以下を参照してください](#join-function-export-arrays).

![結合関数が選択された計算フィールド機能のモーダルウィンドウ。](/help/destinations/assets/ui/export-arrays-calculated-fields/add-calculated-fields-3.png)

を選択 **[!UICONTROL 保存]** 計算フィールドを保持し、マッピングステップに戻ります。

![結合関数が選択され、「保存」コントロールがハイライト表示された計算フィールド機能のモーダルウィンドウ。](/help/destinations/assets/ui/export-arrays-calculated-fields/save-calculated-field.png)

ワークフローのマッピング手順に戻り、を入力します。 **[!UICONTROL ターゲットフィールド]** には、書き出されたファイルのこのフィールドに必要な列ヘッダーの値を指定します。

![ターゲットフィールドが強調表示されたマッピングステップ。](/help/destinations/assets/ui/export-arrays-calculated-fields/fill-in-target-field.png)

![ターゲットフィールド 2 を選択](/help/destinations/assets/ui/export-arrays-calculated-fields/target-field-filled-in.png)

準備ができたら、 **[!UICONTROL 次]** をクリックして、アクティベーションワークフローの次の手順に進みます。

![ターゲットフィールドがハイライト表示されターゲット値が入力されたマッピングステップ。](/help/destinations/assets/ui/export-arrays-calculated-fields/select-next-to-proceed.png)

## サポートされる関数 {#supported-functions}

すべてのドキュメント [データ準備関数](/help/data-prep/functions.md) は、ファイルベースの宛先に対してデータをアクティブ化する際にサポートされます。

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

上記の関数の一部については、以下の節の例と詳細情報を参照してください。 リストされた残りの関数については、を参照してください [データ準備セクションの一般的な関数に関するドキュメント](/help/data-prep/functions.md).

### `join` 配列を書き出す関数 {#join-function-export-arrays}

の使用 `join` 次のような目的の区切り文字を使用して、配列の要素を文字列に連結する関数 `_` または `|`.

例えば、以下の XDM フィールドを、マッピングのスクリーンショットに示すように、を使用して組み合わせることができます。 `join('_',loyalty.loyaltyID)` 構文：

* `"organizations": ["Marketing","Sales,"Finance"]` 配列
* `person.name.firstName` string
* `person.name.lastName` string
* `personalEmail.address` string

![結合関数を含むマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-join-function.png)

この場合、出力ファイルは次のようになります。 を使用して、配列の 3 つの要素が 1 つの文字列に連結される方法に注意してください。 `_` 文字。

```
`First_Name,Last_Name,Personal_Email,Organization
John,Doe,johndoe@acme.org, "Marketing_Sales_Finance"
```

### `iif` 配列を書き出す関数 {#iif-function-export-arrays}

の使用 `iif` 特定の条件下で配列の要素をエクスポートする関数。 例えば、 `organizations` 上記の配列オブジェクトから、次のような単純な条件関数を記述できます。 `iif(organizations[0].equals("Marketing"), "isMarketing", "isNotMarketing")`.

![iif 関数を含むマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-iif-function.png)

この場合、出力ファイルは次のようになります。 この場合、配列の最初の要素はマーケティングなので、その人物はマーケティング部門のメンバーです。

```
`First_Name,Last_Name, Personal_Email, Is_Member_Of_Marketing_Dept
John,Doe, johndoe@acme.org, "isMarketing"
```

### `add_to_array` 配列を書き出す関数 {#add-to-array-function-export-arrays}

の使用 `add_to_array` 書き出された配列に要素を追加する関数 この関数は、 `join` 上記で説明した関数。

続行 `organizations` 上記の配列オブジェクトから、次のような関数を記述できます。 `source: join('_', add_to_array(organizations,"2023"))`2023 年にユーザーが属する組織を返します。

![add_to_array 関数を含むマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-add-to-array-function.png)

この場合、出力ファイルは次のようになります。 を使用して、配列の 3 つの要素が 1 つの文字列に連結される方法に注意してください。 `_` 文字列の末尾には文字と 2023 も追加されます。

```
`First_Name,Last_Name,Personal_Email,Organization_Member_2023
John,Doe, johndoe@acme.org,"Marketing_Sales_Finance_2023"
```

### `coalesce` 配列を書き出す関数 {#coalesce-function-export-arrays}

の使用 `coalesce` 配列の最初の null 以外の要素にアクセスして文字列に書き出す関数。

例えば、以下の XDM フィールドを、マッピングのスクリーンショットに示すように、を使用して組み合わせることができます。 `coalesce(subscriptions.hasPromotion)` 最初のを返す構文 `true` 件中 `false` 配列の値：

* `"subscriptions.hasPromotion": [null, true, null, false, true]` 配列
* `person.name.firstName` string
* `person.name.lastName` string
* `personalEmail.address` string

![Coalesce 関数を含むマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-coalesce-function.png)

この場合、出力ファイルは次のようになります。 最初の null 以外の値 `true` 配列の値はファイルに書き出されます。

```
First_Name,Last_Name,hasPromotion
John,Doe,true
```

### `size_of` 配列を書き出す関数 {#sizeof-function-export-arrays}

の使用 `size_of` 配列内に存在する要素の数を示す関数。 例えば、 `purchaseTime` 複数のタイムスタンプを持つ配列オブジェクトの場合は、 `size_of` ある人物が個別に購入した回数を示す機能。

例えば、マッピングのスクリーンショットに示すように、以下の XDM フィールドを組み合わせることができます。

* `"purchaseTime": ["1538097126","1569633126,"1601255526","1632791526","1664327526"]` お客様による 5 つの個別の購入時間を示す配列
* `personalEmail.address` string

![size_of 関数を含むマッピング例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-size-of-function.png)

この場合、出力ファイルは次のようになります。 2 番目の列が、顧客による個別の購入の数に対応する、配列の要素の数を示していることに注意してください。

```
`Personal_Email,Times_Purchased
johndoe@acme.org,"5"
```

### インデックス・ベースのアレイ・アクセス {#index-based-array-access}

配列のインデックスにアクセスして、配列から 1 つの項目を書き出すことができます。 例えば、上記の例と同様、 `size_of` 機能を使用すると、顧客が特定の製品を初めて購入した場合にのみアクセスして書き出すことを検討している場合は、を使用できます `purchaseTime[0]` タイムスタンプの最初の要素を書き出すには、 `purchaseTime[1]` タイムスタンプの 2 番目の要素を書き出すには、 `purchaseTime[2]` など、タイムスタンプの 3 番目の要素を書き出します。

![配列の要素にアクセスする方法を示すマッピング例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-index.png)

この場合、出力ファイルは次のようになります。顧客が初めて購入したときに書き出されます。

```
`Personal_Email,First_Purchase
johndoe@acme.org,"1538097126"
```

### `first` および `last` 配列を書き出す関数 {#first-and-last-functions-export-arrays}

の使用 `first` および `last` 配列の最初または最後の要素を書き出す関数。 例えば、 `purchaseTime` 前の例で使用されていた複数のタイムスタンプを持つ配列オブジェクトで、これらを使用して、人物による最初または最後の購入時間を書き出す関数を使用できます。

![first 関数と last 関数を含むマッピング例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-first-last-functions.png)

この場合、出力ファイルは次のようになります。顧客が購入を行った最初と最後の時間が書き出されます。

```
`Personal_Email,First_Purchase, Last_Purchase
johndoe@acme.org,"1538097126","1664327526"
```

### ハッシュ関数 {#hashing-functions}

配列または要素を配列から書き出すための関数に加えて、ハッシュ関数を使用して、書き出されたファイルの属性をハッシュ化できます。 例えば、属性に個人を特定できる情報がある場合、それらのフィールドを書き出す際にハッシュ化できます。

文字列値を直接ハッシュ化できます。例： `md5(personalEmail.address)`. 必要に応じて、配列フィールドの要素が次のような文字列であると仮定して、配列フィールドの個々の要素をハッシュ化することもできます。 `md5(purchaseTime[0])`

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