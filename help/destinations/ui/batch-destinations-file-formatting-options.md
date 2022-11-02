---
description: ファイルベースの宛先に対してデータをアクティブ化する際に、ファイル形式オプションを設定する方法を説明します
title: （ベータ版）ファイルベースの宛先のファイル形式設定オプションを設定する
source-git-commit: 23a7a1997e05d2bde26de5b73a23ea051bf2b3bb
workflow-type: tm+mt
source-wordcount: '565'
ht-degree: 20%

---

# （ベータ版）ファイルベースの宛先のファイル形式設定オプションを設定する

>[!IMPORTANT]
>
>この **[!UICONTROL ファイル形式オプション]** Adobe Experience Platformの機能は現在ベータ版です。 ドキュメントと機能は変更される場合があります。
>この機能へのアクセス権については、アドビ担当者にお問い合わせください。
> 
>このドキュメントで説明するファイルフォーマットオプションは、現在、CSV ファイルでのみ使用できます。

書き出したファイルに対して様々なファイル形式設定オプションを設定するオプションは、 [接続](/help/destinations/ui/connect-destination.md) をファイルベースの宛先（例： ）に追加する [Amazon S3](/help/destinations/catalog/cloud-storage/amazon-s3.md#connect), [Azure Blob](/help/destinations/catalog/cloud-storage/azure-blob.md#connect)または [SFTP](/help/destinations/catalog/cloud-storage/sftp.md#connect).

Experience PlatformUI を使用して、書き出したファイルに対して様々なファイル形式設定オプションを設定できます。 Experience Platform から受け取ったファイルを最適に読み取り、解釈するために、書き出されたファイルのいくつかのプロパティを、ユーザー側のファイル受け取りシステムの要件に合わせて変更することができます。

<!--
* To configure file formatting options for exported files by using the Experience Platform UI, read this document.
* To configure file formatting options for exported files by using the Experience Platform Flow Service API, read [Flow Service API - Destinations](https://developer.adobe.com/experience-platform-apis/references/destinations/).
-->

## ファイル形式設定 {#file-configuration}

>[!IMPORTANT]
>
>接続先には、これらのオプションの一部が使用できない場合があります。 宛先でサポートするファイル形式設定オプションは、宛先の開発者が決定します。 宛先の開発者は、宛先に接続する際に使用できるオプションを決定できます。 必須オプションは、Experience PlatformUI でアスタリスクでマークされます。

ファイル形式設定オプションを表示するには、 [宛先に接続](/help/destinations/ui/connect-destination.md) ワークフローとしてのセグメントの選択 **ファイルタイプ**. この節では、書き出したファイルのフォーマット設定について説明します `CSV` ファイル。

![使用可能なファイル形式設定オプションの一部を示す画像。](/help/destinations/assets/ui/batch-destinations-file-formatting-options/file-formatting-options.png)

### 区切り

各フィールドと値の区切り文字を設定します。 例： `,` （コンマ区切り値の場合）または `/t` （タブ区切り値の場合）

### 引用符文字

引用された値をエスケープするために使用する 1 文字を設定します。区切り記号を値の一部として使用することもできます。

### エスケープ文字

既に引用されている値の内部で引用符をエスケープするために使用する 1 文字を設定します。

### 空の値出力

空の値の文字列表現を設定します。

### Null 値出力

書き出されたファイル内の null 値の文字列表現を設定します。

を使用した出力の例 **[!UICONTROL null]** 選択済み： `male,NULL,TestLastName`
を使用した出力の例 **&quot;&quot;** 選択済み： `male,"",TestLastName`
を使用した出力の例 **[!UICONTROL 空の文字列]** 選択済み： `male,,TestLastName`

### 圧縮形式

データをファイルに保存する際に使用する圧縮コーデックを設定します。 GZIP と NONE がサポートされています。

### エンコード

*UI のスクリーンショットには表示されません*. 保存した CSV ファイルのエンコーディング（文字セット）を指定します。 オプションは UTF-8 または UTF-16 です。

### 引用符をエスケープする文字

*UI のスクリーンショットには表示されません*. 引用符を含む値を常に引用符で囲む必要があるかどうかを示すフラグ。

デフォルトでは、引用符文字を含むすべての値をエスケープします。

### 行区切り記号

*UI のスクリーンショットには表示されません*. 書き込みに使用する行区切り記号を定義します。 最大長は 1 文字です。

### 先頭の空白を無視

*UI のスクリーンショットには表示されません*. 書き出される値の先頭の空白をスキップするかどうかを示すフラグ。

を使用した出力の例 **[!UICONTROL True]** 選択済み： `"male","John","TestLastName"`
を使用した出力の例 **[!UICONTROL False]** 選択済み： `" male","John","TestLastName"`

### 末尾の空白を無視

UI のスクリーンショットには表示されません。 書き出す値の末尾の空白をスキップするかどうかを示すフラグです。

を使用した出力の例 **[!UICONTROL True]** 選択済み： `"male","John","TestLastName"`
を使用した出力の例 **[!UICONTROL False]** 選択済み： `"male ","John","TestLastName"`

### 次の手順 {#next-steps}

このドキュメントを読んだ後、CSV データファイルのファイル書き出しオプションを設定し、ダウンストリームのファイル受信システムの要件に合わせてファイルの内容を調整する方法がわかりました。 次に、 [ファイルベースの宛先のアクティベーションチュートリアル](/help/destinations/ui/activate-batch-profile-destinations.md) をクリックして、目的のクラウドストレージの場所へのファイルの書き出しを開始します。
