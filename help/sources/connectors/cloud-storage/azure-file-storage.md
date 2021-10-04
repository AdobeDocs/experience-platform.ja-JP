---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure File Storage;azure ファイルストレージ
solution: Experience Platform
title: Azure ファイルストレージソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して Azure File Storage をAdobe Experience Platformに接続する方法を説明します。
exl-id: 0a5e9df6-9760-4eeb-86d5-d92d77df3d2b
source-git-commit: 1f9948d6e419ee5d6a021a589378f7aa990b7291
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 3%

---

# Azure ファイルストレージコネクタ

Adobe Experience Platformは、AWS、[!DNL Google Cloud Platform]、[!DNL Azure] などのクラウドプロバイダーにネイティブ接続を提供し、これらのシステムからデータを取り込むことができます。

クラウドストレージソースは、ダウンロード、フォーマット、アップロードを必要とせずに、独自のデータを [!DNL Platform] に取り込むことができます。 取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で指定できます。 プロセスの各ステップは、ソースワークフローに統合されます。 [!DNL Platform] では、バッチを使用してからデータを取り込 [!DNL Azure File Storage] むことができます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する可能性があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md) ページを参照してください。

>[!IMPORTANT]
>
>[!DNL Azure File Storage] ソースコネクタは、現在、Platform への同じ領域接続をサポートしていません。 つまり、Azure インスタンスが Platform と同じネットワーク領域を使用している場合、Platform ソースへの接続を確立できません。 現在、クロスリージョン接続のみがサポートされています。 詳しくは、担当のAdobeアカウントマネージャーにお問い合わせください。

## ファイルとディレクトリの命名制約

以下は、クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストです。

- ディレクトリとファイルコンポーネントの名前は 255 文字以下にする必要があります。
- ディレクトリ名とファイル名の末尾にスラッシュ (`/`) を付けることはできません。 指定した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`.
- 無効な URL パス文字は使用できません。 `\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。 また、制御文字（0x00 ～ 0x1F、\u0081 など）など、ASCII 文字や Unicode 文字の一部も使用できません。 HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、Section 2.2 を参照してください。基本規則 ](https://www.ietf.org/rfc/rfc2616.txt) と [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt)。
- 次のファイル名は使用できません。LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, LPT9, COM1, COM2, COM3, COM4, COM5, COM6, COM8, COM9,prn、AUX、NUL、CON、CLOCK$、ドット文字 (.)、2 つのドット文字 (..)。

## [!DNL Azure File Storage] を [!DNL Platform] に接続

以下のドキュメントでは、API またはユーザーインターフェイスを使用して [!DNL Azure File Storage] を [!DNL Platform] に接続する方法について説明します。

### API の使用

- [フローサービス API を使用した Azure File Storage ベース接続の作成](../../tutorials/api/create/cloud-storage/azure-file-storage.md)
- [フローサービス API を使用したクラウドストレージソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/cloud-storage.md)
- [フローサービス API を使用したクラウドストレージソースのデータフローの作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UI での Azure ファイルストレージソース接続の作成](../../tutorials/ui/create/cloud-storage/azure-file-storage.md)
- [UI でのクラウドストレージ接続のデータフローの作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)
