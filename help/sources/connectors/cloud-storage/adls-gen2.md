---
title: Azure Data Lake Storage Gen2 Source コネクタの概要
description: API またはユーザーインターフェイスを使用して Azure Data Lake Storage Gen2 をAdobe Experience Platformに接続する方法について説明します。
exl-id: 424d7278-44d9-4653-82c0-eb21cbb9b623
source-git-commit: 06b2108715ce368ff4ecf5c6c7dd3a327d9f61b1
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 57%

---

# Azure Data Lake Storage Gen2 コネクタ

Adobe Experience Platform には、AWS、[!DNL Google Cloud Platform]、[!DNL Azure] などのクラウドプロバイダーとのネイティブ接続が用意されており、これらのシステムからデータを取り込むことができます。

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードを行う必要なく、独自のデータをExperience Platformに取り込むことができます。 取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で書式設定できます。 プロセスのすべての手順がソースワークフローに統合されます。Experience Platformでは、[!DNL Azure Data Lake Storage Gen2] （ADLS Gen2）からデータをバッチで取り込むことができます。

## IP アドレスの許可リスト

ソースをExperience Platformに接続する前に、地域固有の IP アドレスを許可リストに追加する必要があります。 詳しくは、[Experience Platformへの接続に対する IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

>[!IMPORTANT]
>
>[!DNL Azure Data Lake Storage Gen2] ソースは、Experience Platformへの同じリージョンの接続をサポートしていません。 [!DNL Azure] インスタンスがExperience Platformと同じネットワーク地域を使用している場合、Experience Platform ソースへの接続を確立できません。 現在、クロス地域接続のみがサポートされています。

## ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストを次に示します。

- ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
- ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`
- 無効な URL パス文字は使用できません。`\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、一部の ASCII 文字や Unicode 文字、例えば制御文字（0x00 ～ 0x1F、\u0081 など）も使用できません。HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
- 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

## [!DNL Azure Data Lake Storage Gen2] をExperience Platformに接続

>[!NOTE]
>
>[!DNL Azure Data Lake Storage Gen2] アカウントの作成に使用されるサービスプリンシパルには、少なくとも **Storage Blob Data Reader** の役割がアクセス制御（IAM）から割り当てられている必要があります

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Azure Data Lake Storage Gen2] をExperience Platformに接続する方法について説明しています。

### API の使用

- [Flow Service API [!DNL Azure Data Lake Storage Gen2]  使用したベース接続の作成](../../tutorials/api/create/cloud-storage/adls-gen2.md)
- [Flow Service API を使用して、クラウドストレージソースのデータ構造とコンテンツを探索](../../tutorials/api/explore/cloud-storage.md)
- [Flow Service API を使用して、クラウドストレージソースのデータフローを作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UI でのソ  [!DNL Azure Data Lake Storage Gen2]  ス接続の作成](../../tutorials/ui/create/cloud-storage/adls-gen2.md)
- [UI でクラウドストレージ接続のデータフローを作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)
