---
keywords: Experience Platform；ホーム；人気のトピック；Oracleオブジェクトストレージ；oracleオブジェクトストレージ
solution: Experience Platform
title: Oracleオブジェクトストレージソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用してOracleオブジェクトストレージをAdobe Experience Platformに接続する方法について説明します。
exl-id: 5e8b85c8-9f01-49a6-9556-7b9c7518fb4b
source-git-commit: 1f9948d6e419ee5d6a021a589378f7aa990b7291
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 60%

---

# Oracleオブジェクトストレージコネクタ

Adobe Experience Platformは、AWSなどのクラウドプロバイダーにネイティブの接続を提供します。 [!DNL Google Cloud Platform]を使用すると、これらのシステムから Platform にデータを取り込み、ダウンストリームのサービスや宛先で使用できます。

クラウドストレージのソースは、ダウンロード、フォーマット、アップロードの必要なく、データを Platform に取り込むことができます。取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で書式設定できます。 プロセスのすべての手順がソースワークフローに統合されます。 Platform では、次の場所からデータを取り込むことができます： [!DNL Oracle Object Storage] バッチを使用します。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、 [IP アドレスの許可リスト](../../ip-address-allow-list.md) ドキュメントを参照してください。

## ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストを次に示します。

- ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
- ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`
- 無効な URL パス文字は使用できません。`\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、一部の ASCII 文字や Unicode 文字は使用できません。例えば、制御文字（0x00 ～ 0x1F、\u0081 など）などです。 HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
- 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

## [!DNL Oracle Object Storage] を Platform に接続

以下のドキュメントでは、API またはユーザーインターフェイスを使用してOracleオブジェクトストレージをAdobe Experience Platformに接続する方法に関する情報を提供します。

### API の使用

- [フローサービス API を使用したOracleオブジェクトストレージベース接続の作成](../../tutorials/api/create/cloud-storage/oracle-object-storage.md)
- [Flow Service API を使用して、クラウドストレージソースのデータ構造とコンテンツを探索](../../tutorials/api/explore/cloud-storage.md)
- [Flow Service API を使用して、クラウドストレージソースのデータフローを作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UI でのOracleオブジェクトストレージソース接続の作成](../../tutorials/ui/create/cloud-storage/oracle-object-storage.md)
- [UI でクラウドストレージ接続のデータフローを作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)
