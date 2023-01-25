---
description: ファイルベースの宛先に対してデータをアクティブ化する際に、ファイル形式オプションを設定する方法を説明します
title: （ベータ版）ファイルベースの宛先のファイル形式オプションの設定
exl-id: f59b1952-e317-40ba-81d1-35535e132a72
source-git-commit: 14ce4a11f53ef24b3008b3f775cc926d05ea8f8e
workflow-type: tm+mt
source-wordcount: '601'
ht-degree: 88%

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

## ファイル形式設定 {#file-configuration}

ファイル形式設定オプションを表示するには、 [宛先に接続](/help/destinations/ui/connect-destination.md) ワークフロー。 選択 **データタイプ：セグメント** および **ファイルタイプ：CSV** 書き出したファイルのフォーマット設定を表示するには `CSV` ファイル。

>[!IMPORTANT]
>
>接続先には、これらのオプションの一部が使用できない場合があります。 宛先でサポートするファイル形式オプションは、宛先の開発者が決定します。 宛先の開発者は、宛先に接続する際に使用できるオプションを決定できます。 必須オプションは、Experience Platform UI でアスタリスクでマークされます。
> 
>新しいクラウドストレージの宛先 — [（ベータ版）Amazon S3](/help/destinations/catalog/cloud-storage/amazon-s3.md), [（ベータ版）Azure Blob](/help/destinations/catalog/cloud-storage/azure-blob.md), [（ベータ版）Azure Data Lake Storage Gen2](/help/destinations/catalog/cloud-storage/adls-gen2.md), [（ベータ版）データランディングゾーン](/help/destinations/catalog/cloud-storage/data-landing-zone.md), [（ベータ版）Google Cloud Storage](/help/destinations/catalog/cloud-storage/google-cloud-storage.md), [（ベータ版）SFTP](/help/destinations/catalog/cloud-storage/sftp.md) ：現在、以下で強調表示されている 6 つの CSV オプションのみをサポートしています。

![使用可能なファイル形式オプションの一部を示す画像。](/help/destinations/assets/ui/batch-destinations-file-formatting-options/file-formatting-options.png)

### 区切り {#delimiter}

各フィールドと値の区切り文字を設定します。 利用可能なオプションは次のとおりです。

* コロン `(:)`
* コンマ `(,)`
* パイプ `(|)`
* セミコロン `(;)`
* Tab `(\\t)`

### 引用符文字

引用された値をエスケープするために使用する 1 文字を設定します。区切り記号を値の一部として使用することもできます。

### エスケープ文字

既に引用されている値の内部で引用符をエスケープするために使用する 1 文字を設定します。

### 空の値出力

空の値の文字列表現を設定します。

### Null 値出力

書き出されたファイル内の null 値の文字列表現を設定します。

**[!UICONTROL null]** が選択された出力の例：`male,NULL,TestLastName`
**&quot;&quot;** が選択された出力の例：`male,"",TestLastName`
**[!UICONTROL 空の文字列]**&#x200B;が選択された出力の例：`male,,TestLastName`

### 圧縮形式

データをファイルに保存する際に使用する圧縮コーデックを設定します。 GZIP と NONE がサポートされています。

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
