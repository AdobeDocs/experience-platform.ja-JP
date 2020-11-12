---
keywords: Experience Platform;home;popular topics;Blob;blob;Azure Blob;azure blob
solution: Experience Platform
title: Azure BLOBコネクタ
topic: overview
description: 以下のドキュメントは、APIまたはユーザーインターフェイスを使用してAzure Blobをプラットフォームに接続する方法に関する情報を提供しています。
translation-type: tm+mt
source-git-commit: e0a0b7fc28b8cc85c5140d3840e06e5c7078c307
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 3%

---


# Azure BLOBコネクタ

Adobe Experience Platformは、AWS、 [!DNL Google Cloud Platform]およびなどのクラウドプロバイダーに対してネイティブの接続を提供 [!DNL Azure]します。 これらのシステムのデータをに取り込むことができ [!DNL Platform]ます。

Cloud storage sources can bring your own data into [!DNL Platform] without the need to download, format, or upload. 取り込んだデータは、XDM JSON、XDMパーケー、または区切り文字として形式設定できます。 プロセスの各手順は、Sourcesワークフローに統合されます。 [!DNL Platform] バッチを使用してデータを取り込むこ [!DNL Azure Blob] とができます。

## IPアドレス許可リスト

IPアドレスのリストは、ソースコネクタを使用する前に許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。 詳しくは、「 [IPアドレスの許可リスト](../../ip-address-allow-list.md) 」ページを参照してください。

## ファイルとディレクトリの命名規則

次に、クラウドストレージのファイルやディレクトリに名前を付ける際に考慮する必要がある制約のリストを示します。

- ディレクトリ名とファイルコンポーネント名は255文字以内にする必要があります。
- ディレクトリ名とファイル名の末尾にスラッシュ(`/`)を付けることはできません。 指定した場合は、自動的に削除されます。
- 次の予約済みURL文字は、適切にエスケープする必要があります。 `! * ' ( ) ; : @ & = + $ , / ? % # [ ]`
- 次の文字は使用できません。 `" \ / : | < > * ?`.
- 無効なURLパス文字は使用できません。 NTFSファイル名で有効なコードポイント `\uE000`は、有効なUnicode文字ではありません。 また、制御文字（0x00 ～ 0x1F、\u0081など）のようなASCII文字やUnicode文字も使用できません。 HTTP/1.1でUnicode文字列を扱うルールについては、 [RFC 2616, Section 2.2を参照してください。基本規則](https://www.ietf.org/rfc/rfc2616.txt) と [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt)。
- 次のファイル名は使用できません。LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、prn、AUX、NUL、CON、CLOCK$、ドット文字(.)および2つのドット文字(..)。

## 接続 [!DNL Azure Blob] 先 [!DNL Platform]

以下のドキュメントは、APIまたはユーザーインターフェイスを使用してAzure Blobをプラットフォームに接続する方法に関する情報を提供しています。

### APIの使用

- [Flow Service APIを使用してAzure BLOBコネクタを作成する](../../tutorials/api/create/cloud-storage/blob.md)
- [Flow Service APIを使用したクラウドストレージシステムの調査](../../tutorials/api/explore/cloud-storage.md)
- [Flow Service APIを使用してクラウドストレージデータを収集する](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UIにAzure BLOBソースコネクタを作成する](../../tutorials/ui/create/cloud-storage/blob.md)
- [UIでのクラウドストレージコネクタのデータフローの設定](../../tutorials/ui/dataflow/batch/cloud-storage.md)