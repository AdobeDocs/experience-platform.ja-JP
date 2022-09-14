---
keywords: Experience Platform；ホーム；人気の高いトピック；Blob;BLOB;Azure Blob;Azure BLOB
solution: Experience Platform
title: Azure Blob ソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して Azure Blob をAdobe Experience Platformに接続する方法を説明します。
exl-id: 62adc74f-3570-42c7-9ae6-3ddbc09eccc7
source-git-commit: 251da91844311d08766ee2407ae0b775d4ac6aba
workflow-type: tm+mt
source-wordcount: '469'
ht-degree: 64%

---

# Azure Blob コネクタ

Adobe Experience Platformは、AWSなどのクラウドプロバイダーにネイティブの接続を提供します。 [!DNL Google Cloud Platform]、および [!DNL Azure]. これらのシステムからにデータを取り込むことができます。 [!DNL Platform].

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードを行う必要なく、独自のデータを [!DNL Platform] に取り込むことができます。取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で書式設定できます。 プロセスのすべての手順がソースワークフローに統合されます。[!DNL Platform] を使用すると、次のデータを取り込むことができます： [!DNL Azure Blob] バッチを使用します。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

>[!IMPORTANT]
>
>この [!DNL Azure Blob] ソースは、Experience Platformへの同じ地域の接続をサポートしていません。 Azure インスタンスがExperience Platformと同じネットワーク地域を使用している場合、Experience Platformソースへの接続を確立できません。 Azure East US 2、Azure West Europe、Azure Australia East リージョンは、 [!DNL Azure Blob] ソース。 現在、地域間の接続のみがサポートされています。

## ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストを次に示します。

- ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
- ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`
- 無効な URL パス文字は使用できません。`\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、一部の ASCII 文字や Unicode 文字、例えば制御文字（0x00 ～ 0x1F、\u0081 など）も使用できません。HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
- 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

## 接続 [!DNL Azure Blob] から [!DNL Platform]

以下のドキュメントでは、API またはユーザーインターフェイスを使用して Azure Blob をAdobe Experience Platformに接続する方法に関する情報を提供します。

### API の使用

- [フローサービス API を使用した Azure BLOB ベース接続の作成](../../tutorials/api/create/cloud-storage/blob.md)
- [Flow Service API を使用して、クラウドストレージソースのデータ構造とコンテンツを探索](../../tutorials/api/explore/cloud-storage.md)
- [Flow Service API を使用して、クラウドストレージソースのデータフローを作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UI での Azure BLOB ソース接続の作成](../../tutorials/ui/create/cloud-storage/blob.md)
- [UI でクラウドストレージ接続のデータフローを作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)
