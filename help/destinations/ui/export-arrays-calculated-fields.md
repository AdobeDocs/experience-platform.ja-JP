---
title: （ベータ版）計算フィールドを使用して、フラットスキーマファイルで配列を書き出す
type: Tutorial
description: 計算フィールドを使用して、フラットスキーマファイル内の配列をReal-Time CDPからクラウドストレージの宛先に書き出す方法について説明します。
badge: 「ベータ版」
source-git-commit: b4a18cdf434055be81dacbf19de4dd3e3f229d19
workflow-type: tm+mt
source-wordcount: '1278'
ht-degree: 2%

---


# （ベータ版）計算フィールドを使用して、フラットスキーマファイルで配列を書き出す {#use-calculated-fields-to-export-arrays-in-flat-schema-files}

>[!CONTEXTUALHELP]
>id="platform_destinations_export_arrays_flat_files"
>title="（ベータ版）アレイのエクスポートのサポート"
>abstract="int、string または boolean 値の単純な配列をExperience Platformから目的のクラウドストレージの宛先に書き出します。 一部制限があります。 広範な例とサポートされる関数については、ドキュメントを参照してください。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/export-arrays-calculated-fields.html#examples" text="例"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/export-arrays-calculated-fields.html#known-limitations" text="既知の制限事項"

>[!AVAILABILITY]
>
>* 計算フィールドを使用して配列を書き出す機能は、現在ベータ版です。 ドキュメントと機能は変更される場合があります。

Real-Time CDPの計算フィールドを使用してフラットスキーマファイル内の配列をクラウドストレージの宛先に書き出す方法について説明します。 この機能で有効になる使用例については、このドキュメントをお読みください。

計算フィールドの内容とその理由に関する詳細な情報を取得します。 Data Prep の計算フィールドの概要と、使用可能なすべての関数の詳細については、以下にリンクされているページを参照してください。

* [UI ガイドと概要](/help/data-prep/ui/mapping.md#calculated-fields)
* [データ準備関数](/help/data-prep/functions.md)

>[!IMPORTANT]
>
>上記のすべての関数がサポートされているわけではありません *クラウドストレージの宛先にフィールドを書き出す場合* 計算フィールド機能を使用する。 詳しくは、 [サポートされる関数の節](#supported-functions) 詳しくは、以下を参照してください。

## Platform の配列とその他のオブジェクトタイプ {#arrays-strings-other-objects}

Experience Platformでは、 [XDM スキーマ](/help/xdm/home.md) をクリックして、異なるフィールドタイプを管理します。 以前は、単純なキーと値のペアタイプのフィールド（文字列など）を目的の宛先にExperience Platformから書き出すことができました。 以前の書き出しでサポートされていたフィールドの例を次に示します。 `personalEmail.address`:`johndoe@acme.org`.

Experience Platform内の他のフィールドタイプには、配列フィールドが含まれます。 詳細を表示： [Experience PlatformUI での配列フィールドの管理](/help/xdm/ui/fields/array.md). 以前にサポートされていたフィールドタイプに加えて、次のような配列オブジェクトを書き出せるようになりました。 `organizations:[marketing, sales, engineering]`. 詳しくは、以下を参照してください。 [広範な例](#examples) 様々な関数を使用して配列の要素にアクセスする方法や、配列要素を文字列に結合する方法などを説明します。

## 既知の制限事項 {#known-limitations}

この機能のベータ版リリースに関する既知の制限事項は次のとおりです。

* 現時点では、階層スキーマを使用した JSON または Parquet ファイルへの書き出しはサポートされていません。 配列は、フラットスキーマの CSV、JSON、Parquet の各ファイルにのみ書き出すことができます。
* この時 *単純な配列（またはプリミティブ値の配列）のみをクラウドストレージの宛先に書き出すことができます*. つまり、文字列、整数、ブール値を含む配列オブジェクトを書き出すことができます。 マップまたはマップまたはオブジェクトの配列は書き出せません。計算フィールドのモーダルウィンドウには、書き出し可能な配列のみが表示されます。

## 前提条件 {#prerequisites}

の進行状況 [クラウドストレージの宛先のアクティブ化手順](/help/destinations/ui/activate-batch-profile-destinations.md) そして、にアクセスします。 [マッピング](/help/destinations/ui/activate-batch-profile-destinations.md#mapping) 手順

## 計算フィールドのエクスポート方法 {#how-to-export-calculated-fields}

クラウドストレージの宛先のアクティベーションワークフローのマッピング手順で、「 」を選択します。 **[!UICONTROL （ベータ版）計算フィールドの追加]**.

![エクスポートする計算フィールドを追加](/help/destinations/assets/ui/export-arrays-calculated-fields/add-calculated-fields.png)

モーダルウィンドウが開き、属性を選択してExperience Platformから属性を書き出すことができます。

>[!IMPORTANT]
>
>で使用できるのは、XDM スキーマの一部のフィールドのみです。 **[!UICONTROL フィールド]** 表示。 文字列値、文字列、整数、ブール値の配列を確認できます。 例えば、 `segmentMembership` 配列は他の配列値を含むので、表示されません。

![モーダルウィンドウ 1](/help/destinations/assets/ui/export-arrays-calculated-fields/add-calculated-fields-2.png)

例えば、 `join` 関数を `loyaltyID` フィールドに値を入力し、CSV ファイル内でロイヤルティ ID の配列をアンダースコアと連結された文字列として書き出します。 表示 [この他の例の詳細を以下に示します](#join-function-export-arrays).

![モーダルウィンドウ 2](/help/destinations/assets/ui/export-arrays-calculated-fields/add-calculated-fields-3.png)

選択 **[!UICONTROL 保存]** をクリックして計算フィールドを保持し、マッピングの手順に戻ります。

![モーダルウィンドウ 3](/help/destinations/assets/ui/export-arrays-calculated-fields/save-calculated-field.png)

ワークフローのマッピング手順に戻り、 **[!UICONTROL ターゲットフィールド]** を、エクスポートされたファイルのこのフィールドに必要な列ヘッダーの値に設定します。

![ターゲットフィールド 1 を選択](/help/destinations/assets/ui/export-arrays-calculated-fields/fill-in-target-field.png)

![ターゲットフィールド 2 を選択](/help/destinations/assets/ui/export-arrays-calculated-fields/target-field-filled-in.png)

準備が整ったら、「 」を選択します。 **[!UICONTROL 次へ]** をクリックして、アクティベーションワークフローの次の手順に進みます。

![「次へ」を選択して次に進みます。](/help/destinations/assets/ui/export-arrays-calculated-fields/select-next-to-proceed.png)

## サポートされる  関数 {#supported-functions}

計算フィールドのベータリリースでは、宛先と配列のサポートに対して、次の関数のみがサポートされていることに注意してください。

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

上記の関数の一部については、以下の節の例と詳細情報を参照してください。 上記の残りの関数については、 [「データ準備」セクションの一般的な関数のドキュメント](/help/data-prep/functions.md).

### `join` 配列を書き出す関数 {#join-function-export-arrays}

以下を使用します。 `join` 関数を使用して、目的の区切り文字 ( `_` または `|`.

例えば、マッピングのスクリーンショットに示すように、以下の XDM フィールドを組み合わせるには、 `join('_',loyalty.loyaltyID)` 構文：

* `"organizations": ["Marketing","Sales,"Finance"]` 配列
* `person.name.firstName` string
* `person.name.lastName` string
* `personalEmail.address` string

![マッピングのスクリーンショット](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-join-function.png)

この場合、出力ファイルは次のようになります。 配列の 3 つの要素が、 `_` 文字。

```
`First_Name,Last_Name,Organization
John,Doe,"Marketing_Sales_Finance"
```

### `iif` 配列を書き出す関数 {#iif-function-export-arrays}

以下を使用します。 `iif` 関数を使用して、特定の条件下で配列の要素を書き出すことができます。 例えば、 `organzations` 上から配列オブジェクトを作成すると、 `iif(organizations[0].equals("Marketing"), "isMarketing", "isNotMarketing")`.

![最初と最後の関数のマッピングのスクリーンショット](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-iif-function.png)

この場合、出力ファイルは次のようになります。 この場合、配列の最初の要素は「マーケティング」なので、人物はマーケティング部門のメンバーです。

```
`First_Name,Last_Name, Personal_Email, Is_Member_Of_Marketing_Dept
John,Doe, johndoe@acme.org, "isMarketing"
```

### `coalesce` 配列を書き出す関数 {#coalesce-function-export-arrays}

以下を使用します。 `coalesce` 関数を使用して、配列の最初の null 以外の要素にアクセスし、文字列に書き出します。

例えば、マッピングのスクリーンショットに示すように、以下の XDM フィールドを組み合わせるには、 `coalesce(subscriptions.hasPromotion)` 配列内の false 値の最初の true を返す構文：

* `"subscriptions.hasPromotion": [null, true, null, false, true]` 配列
* `person.name.firstName` string
* `person.name.lastName` string
* `personalEmail.address` string

![coalesce 関数のマッピングスクリーンショット](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-coalesce-function.png)

この場合、出力ファイルは次のようになります。 null 以外の最初の `true` 配列の値がファイルにエクスポートされます。

```
First_Name,Last_Name,hasPromotion
John,Doe,true
```


### `size_of` 配列を書き出す関数 {#sizeof-function-export-arrays}

以下を使用します。 `size_of` 関数を使用して、配列に存在する要素の数を示します。 例えば、 `purchaseTime` 複数のタイムスタンプを持つ配列オブジェクトの場合、 `size_of` 関数を使用して、個別の購入が個別におこなわれた回数を示します。

例えば、マッピングのスクリーンショットに示すように、以下の XDM フィールドを組み合わせることができます。

* `"purchaseTime": ["1538097126","1569633126,"1601255526","1632791526","1664327526"]` 顧客別の 5 回の購入時間を示す配列
* `personalEmail.address` string

![size_of 関数のマッピングスクリーンショット](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-size-of-function.png)

この場合、出力ファイルは次のようになります。 2 番目の列に、顧客が行った個別の購入の数に対応する、配列内の要素の数が示されていることに注意してください。

```
`Personal_Email,Times_Purchased
johndoe@acme.org,"5"
```

### インデックスベースのアレイ・アクセス {#index-based-array-access}

配列のインデックスにアクセスして、配列から単一の項目を書き出すことができます。 例えば、上記の `size_of` 関数を使用する場合、顧客が特定の製品を初めて購入したときにのみアクセスして書き出しをおこなう場合は、 `purchaseTime[0]` タイムスタンプの最初の要素を書き出すには、次の手順を実行します。 `purchaseTime[1]` タイムスタンプの 2 番目の要素を書き出すには、次の手順に従います。 `purchaseTime[2]` タイムスタンプの 3 番目の要素を書き出すなど。

![インデックスにアクセスするためのマッピングスクリーンショット](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-index.png)

この場合、出力ファイルは次のようになります。

```
`Personal_Email,First_Purchase
johndoe@acme.org,"1538097126"
```

### `first` および `last` 配列を書き出す関数 {#first-and-last-functions-export-arrays}

以下を使用します。 `first` および `last` 配列の最初または最後の要素を書き出す関数 例えば、 `purchaseTime` 配列オブジェクトに前の例の複数のタイムスタンプを含める場合、これらの関数を使用して、個人が行った最初または最後の購入時間を書き出すことができます。

![最初と最後の関数のマッピングのスクリーンショット](/help/destinations/assets/ui/export-arrays-calculated-fields/mapping-first-last-functions.png)

この場合、出力ファイルは次のようになります。

```
`Personal_Email,First_Purchase, Last_Purchase
johndoe@acme.org,"1538097126","1664327526"
```

### `md5` および `sha256` ハッシュ関数 {#hashing-functions}

配列または要素を配列から書き出すための関数に加えて、ハッシュ関数を使用して属性をハッシュ化できます。 例えば、属性に個人を特定できる情報が含まれている場合は、それらのフィールドを書き出す際にハッシュ化できます。

文字列値を直接ハッシュ化できます。例： `md5(personalEmail.address)`. 必要に応じて、次のように配列フィールドの個々の要素をハッシュ化することもできます。 `md5(purchaseTime[0])`



