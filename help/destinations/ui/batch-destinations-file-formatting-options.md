---
description: ファイルベースの宛先に対してデータをアクティブ化する際に、ファイル形式オプションを設定する方法を説明します
title: （ベータ版）ファイルベースの宛先のファイル形式オプションの設定
exl-id: f59b1952-e317-40ba-81d1-35535e132a72
source-git-commit: b1e9b781f3b78a22b8b977fe08712d2926254e8c
workflow-type: tm+mt
source-wordcount: '1214'
ht-degree: 41%

---

# （ベータ版）ファイルベースの宛先のファイル形式オプションの設定

>[!IMPORTANT]
>
>Adobe Experience Platform の&#x200B;**[!UICONTROL ファイル形式オプション]**機能は、現在ベータ版です。 ドキュメントと機能は変更される場合があります。
>この機能に対するアクセス権については、アドビ担当者にお問い合わせください。
> 
>このドキュメントで説明するファイル形式オプションは、現在 CSV ファイルでのみ使用できます。

書き出したファイルに対して様々なファイル形式オプションを設定するオプションは、ファイルベースの宛先（[Amazon S3](/help/destinations/catalog/cloud-storage/amazon-s3.md#connect)、[Azure Blob](/help/destinations/catalog/cloud-storage/azure-blob.md#connect) または [SFTP](/help/destinations/catalog/cloud-storage/sftp.md#connect) など）に[接続](/help/destinations/ui/connect-destination.md)するときに使用できます。

Experience Platform UI を使用して、書き出したファイルに対して様々なファイル形式オプションを設定できます。 Experience Platform から受け取ったファイルを最適に読み取り、解釈するために、書き出されたファイルのいくつかのプロパティを、ユーザー側のファイル受け取りシステムの要件に合わせて変更することができます。

<!--
* To configure file formatting options for exported files by using the Experience Platform UI, read this document.
* To configure file formatting options for exported files by using the Experience Platform Flow Service API, read [Flow Service API - Destinations](https://developer.adobe.com/experience-platform-apis/references/destinations/).
-->

## ファイル形式設定 CSV ファイルの場合 {#file-configuration}

ファイル形式設定オプションを表示するには、 [宛先に接続](/help/destinations/ui/connect-destination.md) ワークフロー。 選択 **データタイプ：セグメント** および **ファイルタイプ：CSV** 書き出したファイルのフォーマット設定を表示するには `CSV` ファイル。

>[!IMPORTANT]
>
>接続先には、これらのオプションの一部が使用できない場合があります。 宛先でサポートするファイル形式オプションは、宛先の開発者が決定します。 宛先の開発者は、宛先に接続する際に使用できるオプションを決定できます。 必須オプションは、Experience Platform UI でアスタリスクでマークされます。
> 
>新しいクラウドストレージの宛先 — [（ベータ版）Amazon S3](/help/destinations/catalog/cloud-storage/amazon-s3.md), [（ベータ版）Azure Blob](/help/destinations/catalog/cloud-storage/azure-blob.md), [（ベータ版）Azure Data Lake Storage Gen2](/help/destinations/catalog/cloud-storage/adls-gen2.md), [（ベータ版）データランディングゾーン](/help/destinations/catalog/cloud-storage/data-landing-zone.md), [（ベータ版）Google Cloud Storage](/help/destinations/catalog/cloud-storage/google-cloud-storage.md), [（ベータ版）SFTP](/help/destinations/catalog/cloud-storage/sftp.md) ：現在、以下で強調表示されている 6 つの CSV オプションのみをサポートしています。

![使用可能なファイル形式オプションの一部を示す画像。](/help/destinations/assets/ui/batch-destinations-file-formatting-options/file-formatting-options.png)

### 区切り {#delimiter}

>[!CONTEXTUALHELP]
>id="platform_destinations_csvOptions_delimiter"
>title="区切り"
>abstract="このコントロールを使用して、各フィールドと値の区切り文字を設定します。 各選択の例については、ドキュメントを参照してください。"

このコントロールを使用して、書き出された CSV ファイルの各フィールドと値の区切り文字を設定します。 利用可能なオプションは次のとおりです。

* コロン `(:)`
* コンマ `(,)`
* パイプ `(|)`
* セミコロン `(;)`
* Tab `(\t)`

#### 例

UI の各選択項目と共に、書き出された CSV ファイルのコンテンツの以下の例を参照してください。

* を使用した出力の例 **[!UICONTROL コロン`(:)`]** 選択済み： `male:John:Doe`
* を使用した出力の例 **[!UICONTROL コンマ`(,)`]** 選択済み： `male,John,Doe`
* を使用した出力の例 **[!UICONTROL パイプ`(|)`]** 選択済み： `male|John|Doe`
* を使用した出力の例 **[!UICONTROL セミコロン`(;)`]** 選択済み： `male;John;Doe`
* を使用した出力の例 **[!UICONTROL タブ`(\t)`]** 選択済み： `male \t John \t Doe`

### 引用符文字 {#quote-character}

>[!CONTEXTUALHELP]
>id="platform_destinations_csvOptions_quoteCharacter"
>title="引用符文字"
>abstract="書き出した文字列から二重引用符を削除する場合は、このオプションを使用します。 各選択の例については、ドキュメントを参照してください。"

書き出した文字列から二重引用符を削除する場合は、このオプションを使用します。 利用可能なオプションは次のとおりです。

* **[!UICONTROL Null 文字 (\0000)]**. 書き出した CSV ファイルから二重引用符を削除するには、このオプションを使用します。
* **[!UICONTROL 二重引用符 (&quot;)]**. 書き出した CSV ファイルで二重引用符を保持するには、このオプションを使用します。

#### 例

UI の各選択項目と共に、書き出された CSV ファイルのコンテンツの以下の例を参照してください。

* を使用した出力の例 **[!UICONTROL Null 文字 (\0000)]** 選択済み： `Test,John,LastName`
* を使用した出力の例 **[!UICONTROL 二重引用符 (&quot;)]** 選択済み： `"Test","John","LastName"`

### エスケープ文字 {#escape-character}

>[!CONTEXTUALHELP]
>id="platform_destinations_csvOptions_escapeCharacter"
>title="エスケープ文字"
>abstract="既に引用されている値の内部で引用符をエスケープするために使用する 1 文字を設定します。各選択の例については、ドキュメントを参照してください。"

既に引用符で囲まれた値内の引用符をエスケープするための 1 文字を設定するには、このオプションを使用します。 例えば、このオプションは、文字列の一部が既に二重引用符で囲まれている二重引用符で囲まれた文字列がある場合に便利です。 このオプションは、内側の二重引用符を置き換える文字を決定します。 利用可能なオプションは次のとおりです。

* バックスラッシュ `(\)`
* 一重引用符 `(')`

#### 例

UI の各選択項目と共に、書き出された CSV ファイルのコンテンツの以下の例を参照してください。

* を使用した出力の例 **[!UICONTROL バックスラッシュ`(\)`]** 選択済み： `"Test,\"John\",LastName"`
* を使用した出力の例 **[!UICONTROL 一重引用符`(')`]** 選択済み： `"Test,'"John'",LastName"`

### 空の値出力 {#empty-value-output}

>[!CONTEXTUALHELP]
>id="platform_destinations_csvOptions_emptyValueOutput"
>title="空の値出力"
>abstract="このオプションを使用して、書き出された CSV ファイルで空の値を表す方法を設定します。 各選択の例については、ドキュメントを参照してください。"

空の値の文字列表現を設定するには、このコントロールを使用します。 このオプションは、書き出した CSV ファイルで空の値をどのように表すかを決定します。 利用可能なオプションは次のとおりです。

* **[!UICONTROL null]**
* **&quot;&quot;**
* **[!UICONTROL 空の文字列]**

#### 例

UI の各選択項目と共に、書き出された CSV ファイルのコンテンツの以下の例を参照してください。

* を使用した出力の例 **[!UICONTROL null]** 選択済み： `male,NULL,TestLastName`. この場合、Experience Platformは空の値を null 値に変換します。
* を使用した出力の例 **&quot;&quot;** 選択済み： `male,"",TestLastName`. この場合、Experience Platformは空の値を二重引用符の対に変換します。
* を使用した出力の例 **[!UICONTROL 空の文字列]** 選択済み： `male,,TestLastName`. この場合、Experience Platformは空の値をそのまま維持し、（二重引用符なしで）そのまま書き出します。

>[!TIP]
>
>空の値出力と以下のセクションでの null 値出力の違いは、空の値には実際の空の値が含まれるということです。 NULL 値には値がまったくありません。 空の値はテーブル上の空のガラスと考え、null 値はテーブル上にガラスがまったくないと考えます。

### Null 値出力 {#null-value-output}

>[!CONTEXTUALHELP]
>id="platform_destinations_csvOptions_nullValueOutput"
>title="Null 値出力"
>abstract="書き出されたファイル内の null 値の文字列表現を設定するには、このコントロールを使用します。 各選択の例については、ドキュメントを参照してください。"

書き出されたファイル内の null 値の文字列表現を設定するには、このコントロールを使用します。 このオプションは、書き出した CSV ファイルで null 値をどのように表すかを決定します。 利用可能なオプションは次のとおりです。

* **[!UICONTROL null]**
* **&quot;&quot;**
* **[!UICONTROL 空の文字列]**

#### 例

UI の各選択項目と共に、書き出された CSV ファイルのコンテンツの以下の例を参照してください。

* を使用した出力の例 **[!UICONTROL null]** 選択済み： `male,NULL,TestLastName`. この場合、変換はおこなわれず、CSV ファイルに null 値が含まれます。
* を使用した出力の例 **&quot;&quot;** 選択済み： `male,"",TestLastName`. この場合、Experience Platformは、空の文字列を囲む二重引用符で Null 値を置き換えます。
* を使用した出力の例 **[!UICONTROL 空の文字列]** 選択済み： `male,,TestLastName`. この場合、Experience Platformは、null 値を（二重引用符なしで）空の文字列に置き換えます。

### 圧縮形式 {#compression-format}

>[!CONTEXTUALHELP]
>id="platform_destinations_csvOptions_compressionFormat"
>title="圧縮形式"
>abstract="データをファイルに保存する際に使用する圧縮の種類を設定します。 GZIP と NONE がサポートされています。各選択の例については、ドキュメントを参照してください。"

データをファイルに保存する際に使用する圧縮の種類を設定します。 GZIP と NONE がサポートされています。このオプションは、圧縮ファイルを書き出すかどうかを決定します。

### エンコード

*UI のスクリーンショットには表示されません*。保存した CSV ファイルのエンコーディング（文字セット）を指定します。 オプションは UTF-8 または UTF-16 です。

### 引用符をエスケープする文字

*UI のスクリーンショットには表示されません*。引用符を含む値を、常に引用符で囲む必要があるかどうかを示すフラグ。

デフォルトでは、引用符文字を含むすべての値をエスケープします。

### 行区切り記号

*UI のスクリーンショットには表示されません*。書き込みに使用する行区切り記号を定義します。 最大長は 1 文字です。

### 先頭の空白を無視

*UI のスクリーンショットには表示されません*。書き出される値の先頭の空白をスキップするかどうかを示すフラグ。

**[!UICONTROL True]** が選択された出力の例：`"male","John","TestLastName"`
**[!UICONTROL False]** が選択された出力の例：`" male","John","TestLastName"`

### 末尾の空白を無視

UI のスクリーンショットには表示されません。 書き出す値の末尾の空白をスキップするかどうかを示すフラグ。

**[!UICONTROL True]** が選択された出力の例：`"male","John","TestLastName"`
**[!UICONTROL False]** が選択された出力の例：`"male ","John","TestLastName"`

### 次の手順 {#next-steps}

このドキュメントでは、CSV データファイルのファイル書き出しオプションを設定し、ダウンストリームのファイル受信システムの要件に合わせてファイルの内容を調整する方法を確認しました。 次に、[ファイルベースの宛先のアクティベーションのチュートリアル](/help/destinations/ui/activate-batch-profile-destinations.md)を読んで、目的のクラウドストレージの場所へのファイルの書き出しを開始します。
