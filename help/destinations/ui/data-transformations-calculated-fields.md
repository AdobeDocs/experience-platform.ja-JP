---
title: 計算フィールドを使用したクラウドストレージの宛先への書き出されたデータの変換の実行
type: Tutorial
description: 計算フィールド機能を使用して、クラウドストレージの宛先に書き出されたデータに対して変換を実行する方法について説明します
exl-id: 1e14f964-4c03-4d0c-be8d-c3dcb48a335a
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '1604'
ht-degree: 9%

---

# 計算フィールドを使用したクラウドストレージの宛先への書き出されたデータの変換の実行 {#data-transformation-calculated-fields}

>[!CONTEXTUALHELP]
>id="platform_destinations_export_arrays_flat_files"
>title="計算フィールドを追加"
>abstract="<p>**計算フィールドを追加**&#x200B;コントロールを使用して、クラウドストレージの宛先に書き出されたデータに対して様々なデータ変換を実行します。 例えば、データにハッシュを適用したり、配列を文字列に連結したりできます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/data-transformations-calculated-fields.html?lang=ja#examples" text="例"

>[!AVAILABILITY]
>
>クラウドストレージ宛先に書き出されたデータに対して変換を実行する機能は、一般的に、次の宛先で使用できます：[[!DNL Azure Data Lake Storage Gen2]](../../destinations/catalog/cloud-storage/adls-gen2.md)、[[!DNL Data Landing Zone]](../../destinations/catalog/cloud-storage/data-landing-zone.md)、[[!DNL Google Cloud Storage]](../../destinations/catalog/cloud-storage/google-cloud-storage.md)、[[!DNL Amazon S3]](../../destinations/catalog/cloud-storage/amazon-s3.md)、[[!DNL Azure Blob]](../../destinations/catalog/cloud-storage/azure-blob.md)、[[!DNL SFTP]](../../destinations/catalog/cloud-storage/sftp.md)、および[Destination SDK](/help/destinations/destination-sdk/overview.md)を通じて作成されたカスタムパートナー作成クラウドストレージ宛先。

クラウドストレージの宛先に書き出されたデータに対して様々な変換を実行するには、書き出しワークフローのマッピングステップで計算フィールド機能を使用する必要があります。 計算フィールドについて詳しくは、以下のリンク先のページを参照してください。 これらのページには、データ準備の計算フィールドの概要と、使用可能なすべての関数に関する詳細情報が含まれています。

* [UI ガイドと概要](/help/data-prep/ui/mapping.md#calculated-fields)
* [Data Prep 関数](/help/data-prep/functions.md)

## 前提条件 {#prerequisites}

データ変換に計算フィールドを使用するには：

1. [目的のクラウドストレージの宛先に](/help/destinations/ui/connect-destination.md)接続します。 目的のクラウド宛先に接続する場合は、**[!UICONTROL Export arrays, maps, objects]** [ オプションをオフ ](/help/destinations/ui/export-arrays-maps-objects.md#export-arrays-maps-objects-toggle)に切り替えます。
2. クラウドストレージの宛先[の](/help/destinations/ui/activate-batch-profile-destinations.md) アクティブ化手順を実行し、[ マッピング ](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)手順に進みます。

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
>計算フィールドを使用する場合は、適用するデータ変換関数に加えて、`array_to_string`関数を使用してフィールドを文字列に連結する必要があります。

クラウドストレージ宛先のアクティベーションワークフローのマッピング手順で、**[!UICONTROL Add calculated field]**&#x200B;を選択します。

>[!TIP]
>
>**[!UICONTROL Add calculated field]** コントロールがオフに切り替えられた宛先接続では、**[!UICONTROL Export arrays, maps, and objects]** コントロールが無効になっています。 [詳細情報](/help/destinations/ui/export-arrays-maps-objects.md#export-arrays-maps-objects-toggle)。

![ バッチアクティベーションワークフローのマッピングステップで強調表示された計算フィールドを追加します。](/help/destinations/assets/ui/export-arrays-calculated-fields/add-calculated-fields.png)

モーダルウィンドウが開き、Experience Platformから属性を書き出す関数とフィールドを選択できます。

![関数がまだ選択されていない計算フィールド機能のモーダルウィンドウ。](/help/destinations/assets/ui/export-arrays-calculated-fields/add-calculated-fields-2.png)

例えば、以下に示すように、`array_to_string` フィールドの`organizations`関数を使用して、組織配列を文字列としてCSV ファイルに書き出します。 この例と他の例に関する詳細については、[以下を参照してください](#array-to-string-function-export-arrays)。

![配列から文字列関数を選択した計算フィールド機能のモーダルウィンドウ。](/help/destinations/assets/ui/export-arrays-calculated-fields/add-calculated-fields-3.png)

計算フィールドを保持し、マッピングステップに戻るには、**[!UICONTROL Save]**&#x200B;を選択します。

![配列から文字列への関数が選択され、保存コントロールがハイライト表示された計算フィールド機能のモーダルウィンドウ。](/help/destinations/assets/ui/export-arrays-calculated-fields/save-calculated-field.png)

ワークフローのマッピングステップに戻り、書き出したファイルのこのフィールドに必要な列ヘッダーの値を&#x200B;**[!UICONTROL Target field]**&#x200B;に入力します。

ターゲット フィールドがハイライト表示された![ マッピング ステップ。](/help/destinations/assets/ui/export-arrays-calculated-fields/fill-in-target-field.png)

![ ターゲットフィールド 2](/help/destinations/assets/ui/export-arrays-calculated-fields/target-field-filled-in.png)を選択

準備ができたら、**[!UICONTROL Next]**&#x200B;を選択して、アクティベーションワークフローの次のステップに進みます。

![ ターゲットフィールドがハイライト表示され、ターゲット値が入力されたマッピングステップ。](/help/destinations/assets/ui/export-arrays-calculated-fields/select-next-to-proceed.png)

## データ変換を実行するためのサポートされる関数の例 {#supported-functions}

文書化されているすべての[ データ準備関数](/help/data-prep/functions.md)は、ファイルベースの宛先にデータをアクティブ化する際にサポートされます。

以下の関数は、配列の書き出しの処理やフィールドへのハッシュの適用に特化しており、例とともに文書化されています。

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

## データ変換の実行に使用される関数の例 {#examples}

上記の関数の一部については、以下の節の例と詳細を参照してください。 リストされている関数の残りの部分については、「データ準備」セクション [の](/help/data-prep/functions.md)一般関数のドキュメントを参照してください。

### 配列をエクスポートする`array_to_string`関数 {#array-to-string-function-export-arrays}

`array_to_string`関数を使用して、配列の要素を`_`や`|`などの目的の区切り記号を使用して文字列に連結します。 この関数は、配列の要素をExperience PlatformからCSV ファイルに書き出す場合に便利です。

例えば、`array_to_string('_',organizations)`構文を使用して、次のXDM フィールドをマッピングスクリーンショットに示すように組み合わせることができます。

* `organizations`配列
* `person.name.firstName`文字列
* `person.name.lastName`文字列
* `personalEmail.address`文字列

![array_to_string関数を含むマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-array-to-string-function.png)

この場合、出力ファイルは次のようになります。 配列の要素が`_`文字を使用して単一の文字列に連結される方法に注目してください。

```csv
First_Name,Last_Name,Personal_Email,Organization
John,Doe,johndoe@acme.org, "{'id':123,'orgName':'Acme Inc','founded':1990,'latestInteraction':1708041600000}_{'id':456,'orgName':'Superstar Inc','founded':2004,'latestInteraction':1692921600000}_{'id':789,'orgName':'Energy Corp','founded':2021,'latestInteraction':1725753600000}"
```

### フィルターされた配列をエクスポートする`filterArray`関数 {#filter-array}

`filterArray`関数を使用して、書き出された配列の要素をフィルタリングします。 この関数を、さらに前述の`array_to_string`関数と組み合わせることができます。

上の`organizations`配列オブジェクトを続けて、`array_to_string('_', filterArray(organizations, org -> org.founded > 2021))`のような関数を記述できます。この関数は、2021年以降の`founded`の値を持つ組織を返します。

![filterArray関数の例。](/help/destinations/assets/ui/export-arrays-calculated-fields/filter-array-function.png)

この場合、出力ファイルは次のようになります。 条件を満たす配列の2つの要素が、`_`文字を使用して1つの文字列に連結される方法に注目してください。

```csv
John,Doe,johndoe@acme.org, "{'id':123,'orgName':'Acme Inc','founded':1990,'latestInteraction':1708041600000}_{'id':789,'orgName':'Energy Corp','founded':2021,'latestInteraction':1725753600000}"
```

### 変換済み配列をエクスポートする`transformArray`関数 {#transform-array}

`transformArray`関数を使用して、書き出された配列の要素を変換します。 この関数を、さらに前述の`array_to_string`関数と組み合わせることができます。

上から`organizations`配列オブジェクトを続けて、`array_to_string('_', transformArray(organizations, org -> ucase(org.orgName)))`のような関数を記述し、すべての大文字に変換された組織の名前を返すことができます。

![transformArray関数の例。](/help/destinations/assets/ui/export-arrays-calculated-fields/transform-array-function.png)

この場合、出力ファイルは次のようになります。 配列の3つの要素が、`_`文字を使用して1つの文字列に変換され、連結される方法に注目してください。

```csv
John,Doe,johndoe@acme.org,ACME INC_SUPERSTAR INC_ENERGY CORP
```

### 配列をエクスポートする`iif`関数 {#iif-function-export-arrays}

特定の条件で配列の要素を書き出すには、`iif`関数を使用します。 例えば、上から`organizations`配列オブジェクトを続けると、`iif(organizations[0].equals("Marketing"), "isMarketing", "isNotMarketing")`のような単純な条件付き関数を記述できます。

![iif関数を含むマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-iif-function.png)

この場合、出力ファイルは次のようになります。 この場合、配列の最初の要素はマーケティングなので、その人物はマーケティング部門のメンバーです。

```csv
`First_Name,Last_Name, Personal_Email, Is_Member_Of_Marketing_Dept
John,Doe, johndoe@acme.org, "isMarketing"
```

### 配列をエクスポートする`add_to_array`関数 {#add-to-array-function-export-arrays}

`add_to_array`関数を使用して、書き出された配列に要素を追加します。 この関数を、さらに前述の`array_to_string`関数と組み合わせることができます。

上から`organizations`配列オブジェクトを続けて、`source: array_to_string('_', add_to_array(organizations,"2023"))`のような関数を記述し、2023年の人物がメンバーである組織を返すことができます。

![add_to_array関数を含むマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-add-to-array-function.png)

この場合、出力ファイルは次のようになります。 配列の3つの要素が`_`文字を使用して単一の文字列に連結され、文字列の最後に2023も追加されることに注意してください。

```csv
`First_Name,Last_Name,Personal_Email,Organization_Member_2023
John,Doe, johndoe@acme.org,"Marketing_Sales_Finance_2023"
```

### フラット化された配列を書き出す`flattenArray`関数 {#flatten-array}

`flattenArray`関数を使用して、書き出された多次元配列を統合します。 この関数を、さらに前述の`array_to_string`関数と組み合わせることができます。

上から`organizations`配列オブジェクトを続けて、`array_to_string('_', flattenArray(organizations))`のような関数を記述できます。 `array_to_string`関数は、デフォルトで入力配列を文字列に統合することに注意してください。

結果の出力は、前述の`array_to_string`関数と同じです。

### 配列をエクスポートする`coalesce`関数 {#coalesce-function-export-arrays}

`coalesce`関数を使用して、配列の最初のnull以外の要素にアクセスし、文字列に書き出します。

例えば、`coalesce(subscriptions.hasPromotion)`構文を使用して配列内の`true`値の最初の`false`を返すことで、以下のマッピングのスクリーンショットに示すように、次のXDM フィールドを組み合わせることができます。

* `"subscriptions.hasPromotion": [null, true, null, false, true]`配列
* `person.name.firstName`文字列
* `person.name.lastName`文字列
* `personalEmail.address`文字列

![共用関数を含むマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-coalesce-function.png)

この場合、出力ファイルは次のようになります。 配列の最初のnull以外の`true`値がファイルに書き出される方法に注目してください。

```csv
First_Name,Last_Name,hasPromotion
John,Doe,true
```

### 配列をエクスポートする`size_of`関数 {#sizeof-function-export-arrays}

`size_of`関数を使用して、配列内に存在する要素の数を示します。 例えば、複数のタイムスタンプを持つ`purchaseTime`配列オブジェクトがある場合、`size_of`関数を使用して、ユーザーが別々に購入した回数を示すことができます。

例えば、マッピングスクリーンショットに示すように、以下のXDM フィールドを組み合わせることができます。

* 顧客による5回の購入時間を示す`"purchaseTime": ["1538097126","1569633126,"1601255526","1632791526","1664327526"]` アレイ
* `personalEmail.address`文字列

![size_of関数を含むマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-size-of-function.png)

この場合、出力ファイルは次のようになります。 2番目の列が、お客様が行った個別購入数に対応する配列内の要素の数を示していることに注意してください。

```csv
`Personal_Email,Times_Purchased
johndoe@acme.org,"5"
```

### インデックスベースの配列アクセス {#index-based-array-access}

>[!IMPORTANT]
>
>このページで説明されている他の関数とは異なり、配列の個々の要素を書き出すために、*UIで* コントロールを使用するために&#x200B;**[!UICONTROL Calculated fields]**&#x200B;は必要ありません。

配列のインデックスにアクセスして、配列から1つの項目を書き出すことができます。 例えば、`size_of`関数の上記の例と同様に、顧客が特定の製品を初めて購入した場合にのみアクセスしてエクスポートする場合は、`purchaseTime[0]`を使用してタイムスタンプの最初の要素をエクスポートし、`purchaseTime[1]`を使用してタイムスタンプの2番目の要素をエクスポートし、`purchaseTime[2]`を使用してタイムスタンプの3番目の要素をエクスポートします。

配列の要素へのアクセス方法を示す![ マッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-index.png)

この場合、出力ファイルは次のようになり、顧客が初めて購入した際に書き出されます。

```csv
`Personal_Email,First_Purchase
johndoe@acme.org,"1538097126"
```

### 配列をエクスポートする`first`関数と`last`関数 {#first-and-last-functions-export-arrays}

`first`関数と`last`関数を使用して、配列内の最初または最後の要素を書き出します。 例えば、前の例の複数のタイムスタンプを持つ`purchaseTime`配列オブジェクトを続けて使用すると、これらの関数を使用して、ユーザーが作成した最初または最後の購入時間をエクスポートできます。

![最初と最後の関数を含むマッピングの例。](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-first-last-functions.png)

この場合、出力ファイルは次のようになり、顧客が購入した最初と最後の時刻を書き出します。

```csv
`Personal_Email,First_Purchase, Last_Purchase
johndoe@acme.org,"1538097126","1664327526"
```

### ハッシュ関数 {#hashing-functions}

その他の使用可能な関数は、配列または配列からの要素の書き出しに特化しています。 ハッシュ関数を使用して、書き出したファイルの属性をハッシュ化できます。 例えば、属性に個人を特定できる情報がある場合、書き出す際にそれらのフィールドをハッシュ化することができます。

文字列値を直接ハッシュできます（例：`md5(personalEmail.address)`）。 必要に応じて、配列内の要素が文字列であると仮定して、配列フィールドの個々の要素をハッシュすることもできます（例：`md5(purchaseTime[0])`）

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
