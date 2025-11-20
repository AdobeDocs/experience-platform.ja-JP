---
description: ファイルベースの宛先に対してデータをアクティブ化する際に、ファイル形式オプションを設定する方法を説明します
title: ファイルベースの宛先のファイル形式オプションの設定
exl-id: f59b1952-e317-40ba-81d1-35535e132a72
source-git-commit: 4dd6e8685ff5cc61342b20e971216416918b95da
workflow-type: tm+mt
source-wordcount: '1191'
ht-degree: 47%

---

# ファイルベースの宛先のファイル形式オプションの設定

>[!IMPORTANT]
> 
>このドキュメントで説明するファイル形式オプションは、現在 CSV ファイルでのみ使用できます。

書き出したファイルに対して様々なファイル形式オプションを設定するオプションは、ファイルベースの宛先（[Amazon S3](/help/destinations/catalog/cloud-storage/amazon-s3.md#connect)、[Azure Blob](/help/destinations/catalog/cloud-storage/azure-blob.md#connect) または [SFTP](/help/destinations/catalog/cloud-storage/sftp.md#connect) など）に[接続](/help/destinations/ui/connect-destination.md)するときに使用できます。

Experience Platform UI を使用して、書き出したファイルに対して様々なファイル形式オプションを設定できます。 Experience Platform から受け取ったファイルを最適に読み取り、解釈するために、書き出されたファイルのいくつかのプロパティを、ユーザー側のファイル受け取りシステムの要件に合わせて変更することができます。

<!--
* To configure file formatting options for exported files by using the Experience Platform UI, read this document.
* To configure file formatting options for exported files by using the Experience Platform Flow Service API, read [Flow Service API - Destinations](https://developer.adobe.com/experience-platform-apis/references/destinations/).
-->

## CSV ファイルのファイル形式設定 {#file-configuration}

ファイル形式オプションを表示するには、[&#x200B; 宛先に接続 &#x200B;](/help/destinations/ui/connect-destination.md) ワークフローを開始します。 **データタイプ：セグメント** および **ファイルタイプ：CSV** を選択して、書き出された `CSV` ファイルで使用できるファイル形式設定を表示します。

>[!IMPORTANT]
>
>接続先には、これらのオプションの一部が使用できない場合があります。 宛先でサポートするファイル形式オプションは、宛先の開発者が決定します。 宛先の開発者は、宛先に接続する際に使用できるオプションを決定できます。 必須オプションは、Experience Platform UI でアスタリスクでマークされます。
> 
>Adobeが構築したクラウドストレージの宛先（[Amazon S3](/help/destinations/catalog/cloud-storage/amazon-s3.md)、[Azure Blob](/help/destinations/catalog/cloud-storage/azure-blob.md)、[Azure Data Lake Storage Gen2](/help/destinations/catalog/cloud-storage/adls-gen2.md)、[Data Landing Zone](/help/destinations/catalog/cloud-storage/data-landing-zone.md)、[Google Cloud Storage](/help/destinations/catalog/cloud-storage/google-cloud-storage.md)、[SFTP](/help/destinations/catalog/cloud-storage/sftp.md)）では、現在、以下にハイライト表示されている 6 つの CSV オプションのみがサポートされています。

![使用可能なファイル形式オプションの一部を示す画像。](../assets/ui/batch-destinations-file-formatting-options/file-formatting-options.png)

### 区切り {#delimiter}

>[!CONTEXTUALHELP]
>id="platform_destinations_csvOptions_delimiter"
>title="区切り"
>abstract="このコントロールを使用して、各フィールドと値の区切り記号を設定します。各選択の例については、ドキュメントを参照してください。"

このコントロールを使用して、書き出された CSV ファイルの各フィールドおよび値の区切り文字を設定します。 利用可能なオプションは次のとおりです。

* 結腸 `(:)`
* コンマ `(,)`
* パイプ `(|)`
* セミコロン `(;)`
* Tab `(\t)`

#### 例

書き出された CSV ファイルのコンテンツの以下の例と、UI での各選択項目を表示します。

* **[!UICONTROL Colon `(:)`]** を選択した場合の出力例：`male:John:Doe`
* **[!UICONTROL Comma `(,)`]** を選択した場合の出力例：`male,John,Doe`
* **[!UICONTROL Pipe `(|)`]** を選択した場合の出力例：`male|John|Doe`
* **[!UICONTROL Semicolon `(;)`]** を選択した場合の出力例：`male;John;Doe`
* **[!UICONTROL Tab `(\t)`]** を選択した場合の出力例：`male \t John \t Doe`

### 引用符文字 {#quote-character}

>[!CONTEXTUALHELP]
>id="platform_destinations_csvOptions_quoteCharacter"
>title="引用符文字"
>abstract="書き出した文字列から二重引用符を削除する場合は、このオプションを使用します。各選択の例については、ドキュメントを参照してください。"

このオプションを使用して、二重引用符を削除するか、書き出された文字列の中に保持するかを制御します。

使用できる選択肢は次のとおりです。

* **[!UICONTROL Null Character (\0000)]**。書き出された CSV ファイルから二重引用符を削除するには、このオプションを使用します。
* **[!UICONTROL Double Quotes (")]**。このオプションは、文字列値に区切り文字または二重引用符が含まれている場合に使用します。 このオプションを使用すると、書き出された CSV ファイルに区切り文字や二重引用符を保持できるので、どの値がどのフィールドに対応するかを正しく識別できます。

#### 例

入力値 `Anna,"Doe,John"` について考えてみます。

書き出された CSV ファイルのコンテンツの以下の例と、UI での各選択項目を表示します。

* **[!UICONTROL Null Character (\0000)]** を選択した場合の出力例：`Anna,Doe,John`
* **[!UICONTROL Double Quotes (")]** を選択した場合の出力例：`Anna,"Doe,John"`

### エスケープ文字 {#escape-character}

>[!CONTEXTUALHELP]
>id="platform_destinations_csvOptions_escapeCharacter"
>title="エスケープ文字"
>abstract="既に引用されている値の内部で引用符をエスケープするために使用する 1 文字を設定します。各選択の例については、ドキュメントを参照してください。"

既に引用されている値の内部で引用符をエスケープする 1 文字を設定するには、このオプションを使用します。 例えば、このオプションは、文字列の一部が既に二重引用符で囲まれている文字列を二重引用符で囲んでいる場合に便利です。 このオプションは、内側の二重引用符を置き換える文字を決定します。 利用可能なオプションは次のとおりです。

* バックスラッシュ `(\)`
* 一重引用符 `(')`

#### 例

書き出された CSV ファイルのコンテンツの以下の例と、UI での各選択項目を表示します。

* **[!UICONTROL Back slash `(\)`]** を選択した場合の出力例：`"Test,\"John\",LastName"`
* **[!UICONTROL Single quote `(')`]** を選択した場合の出力例：`"Test,'"John'",LastName"`

### 空の値出力 {#empty-value-output}

>[!CONTEXTUALHELP]
>id="platform_destinations_csvOptions_emptyValueOutput"
>title="空の値出力"
>abstract="このオプションを使用して、書き出された CSV ファイルで空の値を表す方法を設定します。各選択の例については、ドキュメントを参照してください。"

このコントロールを使用して、空の値の文字列表現を設定します。 このオプションは、書き出された CSV ファイルで空の値を表す方法を決定します。 利用可能なオプションは次のとおりです。

* **[!UICONTROL Null (null)]**
* **二重引用符（&quot;）で囲まれた空の文字列**
* **[!UICONTROL Empty string]**

#### 例

書き出された CSV ファイルのコンテンツの以下の例と、UI での各選択項目を表示します。

* **[!UICONTROL null]** を選択した場合の出力例：`male,NULL,TestLastName`。 この場合、Experience Platformは空の値を null 値に変換します。
* **&quot;&quot;** を選択した場合の出力例：`male,"",TestLastName`。 この場合、Experience Platformは空の値を二重引用符のペアに変換します。
* **[!UICONTROL Empty string]** を選択した場合の出力例：`male,,TestLastName`。 この場合、Experience Platformは空の値を保持し、そのまま（二重引用符なしで）書き出します。

>[!TIP]
>
>以下の節で示す空の値出力と null 値の出力の違いは、空の値には空の実際の値が含まれることです。 NULL 値には値がありません。 空の値はテーブル上に空のガラスがあり、null 値はテーブルにガラスがまったくない状態と考えてください。

### Null 値出力 {#null-value-output}

>[!CONTEXTUALHELP]
>id="platform_destinations_csvOptions_nullValueOutput"
>title="Null 値出力"
>abstract="このコントロールを使用して、書き出されたファイル内の null 値の文字列表現を設定します。各選択の例については、ドキュメントを参照してください。"

このコントロールを使用して、書き出されたファイル内の null 値の文字列表現を設定します。このオプションは、書き出された CSV ファイルで null 値を表す方法を決定します。 利用可能なオプションは次のとおりです。

* **[!UICONTROL Null (null)]**
* **二重引用符（&quot;）で囲まれた空の文字列**
* **[!UICONTROL Empty string]**

#### 例

書き出された CSV ファイルのコンテンツの以下の例と、UI での各選択項目を表示します。

* **[!UICONTROL null]** を選択した場合の出力例：`male,NULL,TestLastName`。 この場合、変換は行われず、CSV ファイルに null 値が含まれます。
* **&quot;&quot;** を選択した場合の出力例：`male,"",TestLastName`。 この場合、Experience Platformは null 値を空の文字列を二重引用符で囲んで置き換えます。
* **[!UICONTROL Empty string]** を選択した場合の出力例：`male,,TestLastName`。 この場合、Experience Platformは null 値を空の文字列（二重引用符なし）に置き換えます。

### 圧縮形式 {#compression-format}

>[!CONTEXTUALHELP]
>id="platform_destinations_csvOptions_compressionFormat"
>title="圧縮形式"
>abstract="データをファイルに保存する際に使用する圧縮タイプを設定します。GZIP と NONE がサポートされています。各選択の例については、ドキュメントを参照してください。"

データをファイルに保存する際に使用する圧縮タイプを設定します。GZIP と NONE がサポートされています。このオプションは、圧縮ファイルを書き出すかどうかを決定します。

### エンコード

*UI のスクリーンショットには表示されません*。保存した CSV ファイルのエンコーディング（文字セット）を指定します。 オプションは UTF-8 または UTF-16 です。

### 引用符をエスケープする文字

*UI のスクリーンショットには表示されません*。引用符を含む値を、常に引用符で囲む必要があるかどうかを示すフラグ。

デフォルトでは、引用符文字を含むすべての値をエスケープします。

### 行区切り記号

*UI のスクリーンショットには表示されません*。書き込みに使用する行区切り記号を定義します。 最大長は 1 文字です。

### 先頭の空白を無視

*UI のスクリーンショットには表示されません*。書き出される値の先頭の空白をスキップするかどうかを示すフラグ。

**[!UICONTROL True]** を選択した場合の出力例：`"male","John","TestLastName"`
**[!UICONTROL False]** を選択した場合の出力例：`" male","John","TestLastName"`

### 末尾の空白を無視

UI のスクリーンショットには表示されません。 書き出す値の末尾の空白をスキップするかどうかを示すフラグ。

**[!UICONTROL True]** を選択した場合の出力例：`"male","John","TestLastName"`
**[!UICONTROL False]** を選択した場合の出力例：`"male ","John","TestLastName"`

### 次の手順 {#next-steps}

このドキュメントでは、CSV データファイルのファイル書き出しオプションを設定し、ダウンストリームのファイル受信システムの要件に合わせてファイルの内容を調整する方法を確認しました。 次に、[ファイルベースの宛先のアクティベーションのチュートリアル](/help/destinations/ui/activate-batch-profile-destinations.md)を読んで、目的のクラウドストレージの場所へのファイルの書き出しを開始します。
