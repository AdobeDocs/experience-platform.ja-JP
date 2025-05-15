---
title: 計算フィールドを使用したクラウドストレージの宛先への書き出されたデータの変換の実行
type: Tutorial
description: 計算フィールド機能を使用して、クラウドストレージの宛先に書き出されたデータに対して変換を実行する方法を説明します
exl-id: 1e14f964-4c03-4d0c-be8d-c3dcb48a335a
source-git-commit: bd9efc1bcf6058827cc5c603b9976c9e42c7ec9e
workflow-type: tm+mt
source-wordcount: '1625'
ht-degree: 8%

---

# 計算フィールドを使用したクラウドストレージの宛先への書き出されたデータの変換の実行 {#data-transformation-calculated-fields}

>[!CONTEXTUALHELP]
>id="platform_destinations_export_arrays_flat_files"
>title="計算フィールドを追加"
>abstract="<p>**計算フィールドを追加**&#x200B;コントロールを使用して、クラウドストレージの宛先に書き出されたデータに対して様々なデータ変換を実行します。 例えば、データにハッシュを適用したり、配列を文字列に連結したりできます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/data-transformations-calculated-fields.html#examples" text="例"

>[!AVAILABILITY]
>
>クラウドストレージの宛先に書き出したデータに対して変換を実行する機能は、[[!DNL Azure Data Lake Storage Gen2]](../../destinations/catalog/cloud-storage/adls-gen2.md)、[[!DNL Data Landing Zone]](../../destinations/catalog/cloud-storage/data-landing-zone.md)、[[!DNL Google Cloud Storage]](../../destinations/catalog/cloud-storage/google-cloud-storage.md)、[[!DNL Amazon S3]](../../destinations/catalog/cloud-storage/amazon-s3.md)、[[!DNL Azure Blob]](../../destinations/catalog/cloud-storage/azure-blob.md)、[[!DNL SFTP]](../../destinations/catalog/cloud-storage/sftp.md) および [Destination SDK](/help/destinations/destination-sdk/overview.md) で作成したパートナーがカスタムで作成したクラウドストレージの宛先で一般に使用できます。

クラウドストレージの宛先に書き出されたデータに対して様々な変換を実行するには、書き出しワークフローのマッピング手順で計算フィールド機能を使用する必要があります。 計算フィールドについて詳しくは、以下にリンクされているページを参照してください。 これらのページには、データ準備の計算フィールドの概要と、使用可能なすべての関数の詳細が含まれています。

* [UI ガイドと概要](/help/data-prep/ui/mapping.md#calculated-fields)
* [Data Prep 関数](/help/data-prep/functions.md)

## 前提条件 {#prerequisites}

データ変換に計算フィールドを使用するには：

1. 目的のクラウドストレージの宛先に [ 接続 ](/help/destinations/ui/connect-destination.md) します。 目的のクラウドの宛先に接続する場合は、**[!UICONTROL 配列、マップ、オブジェクトを書き出し]**[ オプションをオフ ](/help/destinations/ui/export-arrays-maps-objects.md##export-arrays-maps-objects-toggle) に切り替えます。
2. クラウドストレージ宛先の [ アクティベーション手順 ](/help/destinations/ui/activate-batch-profile-destinations.md) を実行し、[ マッピング ](/help/destinations/ui/activate-batch-profile-destinations.md#mapping) 手順に進みます。

## 計算フィールドの操作方法 {#how-to-export-calculated-fields}

>[!CONTEXTUALHELP]
>id="platform_destinations_export_arrays_control"
>title="階層出力スキーマの有効化"
>abstract="この設定をオンに切り替えると、配列、マップ、オブジェクトを JSON または Parquet ファイルに書き出すことができます。"

>[!CONTEXTUALHELP]
>id="platform_destinations_export_arrays_calculated_field_disabled"
>title="計算フィールドの追加の無効化"
>abstract="この宛先接続を設定する際に&#x200B;**配列、マップ、オブジェクトを書き出し**&#x200B;切替スイッチを&#x200B;*オン*&#x200B;にしたので、このコントロールは無効になっています。計算フィールドとその中で使用可能な関数を使用するには、**配列、マップ、オブジェクトを書き出し**&#x200B;切替スイッチを&#x200B;*オフ*&#x200B;にして、新しい宛先接続を設定します。"

>[!IMPORTANT]
>
>計算フィールドを操作するときは、適用するデータ変換関数に加えて、`array_to_string` 関数を使用してフィールドを文字列に連結する必要があります。

クラウドストレージ宛先のアクティベーションワークフローのマッピング手順で、「**[!UICONTROL 計算フィールドを追加]**」を選択します。

>[!TIP]
>
>**[!UICONTROL 配列、マップ、およびオブジェクトの書き出し]** コントロールがオフになっている宛先接続では、**[!UICONTROL 計算フィールドを追加]** コントロールは無効になっています。 [詳細情報](/help/destinations/ui/export-arrays-maps-objects.md#export-arrays-maps-objects-toggle)。

![ バッチアクティベーションワークフローのマッピング手順でハイライト表示された計算フィールドを追加。](/help/destinations/assets/ui/export-arrays-calculated-fields/add-calculated-fields.png)

これにより、Experience Platformから属性を書き出す関数とフィールドを選択できるモーダルウィンドウが開きます。

![ 関数がまだ選択されていない計算フィールド機能のモーダルウィンドウ。](/help/destinations/assets/ui/export-arrays-calculated-fields/add-calculated-fields-2.png)

例えば、以下に示すように、「`organizations`」フィールドで `array_to_string` 関数を使用して、組織の配列を CSV ファイルの文字列として書き出します。 [ この例およびその他の例の詳細は以下 ](#array-to-string-function-export-arrays) をご覧ください。

![ 文字列配列関数が選択された計算フィールド機能のモーダルウィンドウ。](/help/destinations/assets/ui/export-arrays-calculated-fields/add-calculated-fields-3.png)

「**[!UICONTROL 保存]**」を選択して計算フィールドを保持し、マッピングステップに戻ります。

![ 文字列配列関数が選択され、「保存」コントロールがハイライト表示された計算フィールド機能のモーダルウィンドウ。](/help/destinations/assets/ui/export-arrays-calculated-fields/save-calculated-field.png)

ワークフローのマッピング手順に戻り、書き出されたファイルのこのフィールドに必要な列ヘッダーの値を **[!UICONTROL ターゲットフィールド]** に入力します。

![ ターゲットフィールドがハイライト表示されたマッピングステップ ](/help/destinations/assets/ui/export-arrays-calculated-fields/fill-in-target-field.png)

![ ターゲットフィールド 2 を選択 ](/help/destinations/assets/ui/export-arrays-calculated-fields/target-field-filled-in.png)

準備ができたら、「**[!UICONTROL 次へ]** を選択して、アクティベーションワークフローの次のステップに進みます。

![ ターゲットフィールドがハイライト表示され、ターゲット値が入力されたマッピングステップ ](/help/destinations/assets/ui/export-arrays-calculated-fields/select-next-to-proceed.png)

## データ変換を実行するためにサポートされている関数のサンプル {#supported-functions}

ファイルベースの宛先に対してデータをアクティブ化する場合、ドキュメントに記載されているすべての [ データ準備関数 ](/help/data-prep/functions.md) がサポートされます。

以下の関数は、配列の書き出しの処理やフィールドへのハッシュ化の適用に固有の関数で、例とともに説明しています。

* `array_to_string`
* `flattenArray`
* `filterArray`
* `transformArray`
* `coalesce`
* `size_of`
* `iif`
* `index-based array access`
* `add_to_array`
* `to_array`
* `first`
* `last`

## データ変換の実行に使用する関数の例 {#examples}

上記の関数の一部については、以下の節の例と詳細情報を参照してください。 リストに表示されたその他の関数については、[ データ準備セクションの汎用関数のドキュメント ](/help/data-prep/functions.md) を参照してください。

### 配列を書き出す `array_to_string` 関数 {#array-to-string-function-export-arrays}

`array_to_string` 関数を使用すると、`_` や `|` などの目的の区切り文字を使用して、配列の要素を文字列に連結できます。 この関数は、Experience Platformから CSV ファイルに配列の要素を書き出す場合に便利です。

例えば、`array_to_string('_',organizations)` 構文を使用して、マッピングのスクリーンショットに示すように、以下の XDM フィールドを組み合わせることができます。

* `organizations` 配列
* `person.name.firstName` 文字列
* `person.name.lastName` 文字列
* `personalEmail.address` 文字列

![array_to_string 関数を含むマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-array-to-string-function.png)

この場合、出力ファイルは次のようになります。 `_` 文字を使用して、配列の要素が単一の文字列に連結される方法に注意してください。

```
First_Name,Last_Name,Personal_Email,Organization
John,Doe,johndoe@acme.org, "{'id':123,'orgName':'Acme Inc','founded':1990,'latestInteraction':1708041600000}_{'id':456,'orgName':'Superstar Inc','founded':2004,'latestInteraction':1692921600000}_{'id':789,'orgName':'Energy Corp','founded':2021,'latestInteraction':1725753600000}"
```

### フィルターされた配列を書き出す `filterArray` 関数 {#filter-array}

書き出された配列の要素をフィルターするには、`filterArray` 関数を使用します。 この関数は、上記の `array_to_string` 関数と組み合わせることができます。

上記の `organizations` 配列オブジェクトを続けて、`array_to_string('_', filterArray(organizations, org -> org.founded > 2021))` のような関数を記述すると、2021 年以降の `founded` の値を持つ組織が返されます。

![filterArray 関数の例 ](/help/destinations/assets/ui/export-arrays-calculated-fields/filter-array-function.png)

この場合、出力ファイルは次のようになります。 条件を満たす配列の 2 つの要素が、`_` 文字を使用して単一の文字列に連結される方法に注意してください。

```
John,Doe,johndoe@acme.org, "{'id':123,'orgName':'Acme Inc','founded':1990,'latestInteraction':1708041600000}_{'id':789,'orgName':'Energy Corp','founded':2021,'latestInteraction':1725753600000}"
```

### 変換された配列を書き出す `transformArray` 関数 {#transform-array}

`transformArray` 関数を使用して、書き出された配列の要素を変換します。 この関数は、上記の `array_to_string` 関数と組み合わせることができます。

上記の `organizations` 配列オブジェクトを続けて、`array_to_string('_', transformArray(organizations, org -> ucase(org.orgName)))` のような関数を記述し、組織の名前をすべて大文字に変換して返すことができます。

![transformArray 関数の例。](/help/destinations/assets/ui/export-arrays-calculated-fields/transform-array-function.png)

この場合、出力ファイルは次のようになります。 `_` 文字を使用して、配列の 3 つの要素が変換され、1 つの文字列に連結される方法に注意してください。

```
John,Doe,johndoe@acme.org,ACME INC_SUPERSTAR INC_ENERGY CORP
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

書き出された配列に要素を追加するには、`add_to_array` 関数を使用します。 この関数は、上記の `array_to_string` 関数と組み合わせることができます。

上記の `organizations` 配列オブジェクトを続けて、`source: array_to_string('_', add_to_array(organizations,"2023"))` のような関数を記述して、2023 年にユーザーが属する組織を返すことができます。

![add_to_array 関数を含むマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-add-to-array-function.png)

この場合、出力ファイルは次のようになります。 配列の 3 つの要素が、`_` 文字を使用して 1 つの文字列に連結され、文字列の末尾にも 2023 が追加されていることに注意してください。

```
`First_Name,Last_Name,Personal_Email,Organization_Member_2023
John,Doe, johndoe@acme.org,"Marketing_Sales_Finance_2023"
```

### フラット化された配列を書き出すための `flattenArray` 関数 {#flatten-array}

`flattenArray` 関数を使用して、書き出された多次元配列をフラット化します。 この関数は、上記の `array_to_string` 関数と組み合わせることができます。

上記の `organizations` 配列オブジェクトを続けて、`array_to_string('_', flattenArray(organizations))` のような関数を記述できます。 `array_to_string` 関数は、デフォルトで入力配列を文字列にフラット化します。

結果の出力は、前述の `array_to_string` 関数と同じです。

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

>[!IMPORTANT]
>
>このページで説明している他の関数とは異なり、配列の個々の要素を書き出すには、UI で *計算フィールド&#x200B;**[!UICONTROL コントロールを使用する* 必要はありません]**。

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

その他の使用可能な関数は、配列または要素を配列から書き出す場合に固有です。 ハッシュ関数を使用して、書き出されたファイルの属性をハッシュ化できます。 例えば、属性に個人を特定できる情報がある場合、それらのフィールドを書き出す際にハッシュ化できます。

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
