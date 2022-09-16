---
keywords: Experience Platform；ホーム；人気の高いトピック；Azure File Storage;azure ファイルストレージ
solution: Experience Platform
title: Azure ファイルストレージソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して Azure File Storage をAdobe Experience Platformに接続する方法を説明します。
exl-id: 0a5e9df6-9760-4eeb-86d5-d92d77df3d2b
source-git-commit: 446436346e3368d98eb990dba1000ac0974b84dc
workflow-type: tm+mt
source-wordcount: '409'
ht-degree: 83%

---

# Azure File Storage コネクタ

Adobe Experience Platform には、AWS、[!DNL Google Cloud Platform]、[!DNL Azure] などのクラウドプロバイダーとのネイティブ接続が用意されており、これらのシステムからデータを取り込むことができます。

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードを行う必要なく、独自のデータを [!DNL Platform] に取り込むことができます。取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で書式設定できます。 プロセスのすべての手順がソースワークフローに統合されます。[!DNL Platform] を使用すると、次のデータを取り込むことができます： [!DNL Azure File Storage] バッチを使用します。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストを次に示します。

- ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
- ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`
- 無効な URL パス文字は使用できません。`\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、一部の ASCII 文字や Unicode 文字、例えば制御文字（0x00 ～ 0x1F、\u0081 など）も使用できません。HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
- 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

## 接続 [!DNL Azure File Storage] から [!DNL Platform]

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Azure File Storage] と を接続する方法について説明します。[!DNL Platform]

### API の使用

- [フローサービス API を使用した Azure ファイルストレージベース接続の作成](../../tutorials/api/create/cloud-storage/azure-file-storage.md)
- [Flow Service API を使用して、クラウドストレージソースのデータ構造とコンテンツを探索](../../tutorials/api/explore/cloud-storage.md)
- [Flow Service API を使用して、クラウドストレージソースのデータフローを作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UI での Azure ファイルストレージソース接続の作成](../../tutorials/ui/create/cloud-storage/azure-file-storage.md)
- [UI でクラウドストレージ接続のデータフローを作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)
