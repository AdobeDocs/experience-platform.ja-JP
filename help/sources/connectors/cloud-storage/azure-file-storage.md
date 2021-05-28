---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure File Storage;azureファイルストレージ
solution: Experience Platform
title: Azureファイルストレージソースコネクタの概要
topic-legacy: overview
description: APIまたはユーザーインターフェイスを使用してAzure File StorageをAdobe Experience Platformに接続する方法を説明します。
exl-id: 0a5e9df6-9760-4eeb-86d5-d92d77df3d2b
source-git-commit: e150f05df2107d7b3a2e95a55dc4ad072294279e
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 3%

---

# Azureファイルストレージコネクタ

Adobe Experience Platformは、AWS、[!DNL Google Cloud Platform]、[!DNL Azure]などのクラウドプロバイダーにネイティブ接続を提供し、これらのシステムからデータを取り込むことができます。

クラウドストレージソースは、ダウンロード、フォーマット、アップロードを必要とせずに、独自のデータを[!DNL Platform]に取り込むことができます。 取り込んだデータは、XDM JSON形式、XDM Parquet形式または区切り形式で指定できます。 プロセスの各手順は、ソースワークフローに統合されます。 [!DNL Platform] では、バッチを使用してからデータを取り込む [!DNL Azure File Storage] ことができます。

## IPアドレス許可リスト

ソースコネクタを操作する前に、IPアドレスのリストを許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加しないと、ソースを使用する際にエラーやパフォーマンスが低下する可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)のページを参照してください。

>[!IMPORTANT]
>
>[!DNL Azure File Storage]ソースコネクタは、現在、Platformへの同じ領域接続をサポートしていません。 つまり、AzureインスタンスがPlatformと同じネットワーク領域を使用している場合、Platformソースへの接続を確立できません。 現在、クロスリージョン接続のみがサポートされています。 詳しくは、担当のAdobeアカウントマネージャーにお問い合わせください。

## ファイルとディレクトリの命名の制約

以下は、クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストです。

- ディレクトリ名とファイルコンポーネント名は255文字以内にする必要があります。
- ディレクトリ名とファイル名の末尾にスラッシュ(`/`)を付けることはできません。 指定した場合、自動的に削除されます。
- 次の予約URL文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`.
- 無効なURLパス文字は使用できません。 `\uE000`のようなコードポイントは、NTFSファイル名では有効ですが、有効なUnicode文字ではありません。 また、制御文字（0x00～0x1F、\u0081など）のようなASCII文字やUnicode文字も使用できません。 HTTP/1.1でUnicode文字列を規定するルールについては、[RFC 2616、2.2節を参照してください。基本規則](https://www.ietf.org/rfc/rfc2616.txt)と[RFC 3987](https://www.ietf.org/rfc/rfc3987.txt)。
- 次のファイル名は使用できません。LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、prn、AUX、NUL、CON、CLOCK$、ドット文字(.)、2つのドット文字(..)。

## [!DNL Azure File Storage]を[!DNL Platform]に接続します

以下のドキュメントでは、APIまたはユーザーインターフェイスを使用して[!DNL Azure File Storage]を[!DNL Platform]に接続する方法について説明します。

### API の使用

- [フローサービスAPIを使用したAzureファイルストレージソース接続の作成](../../tutorials/api/create/cloud-storage/azure-file-storage.md)
- [フローサービスAPIを使用したクラウドストレージシステムの調査](../../tutorials/api/explore/cloud-storage.md)
- [フローサービスAPIを使用したクラウドストレージデータの収集](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UIでのAzureファイルストレージソース接続の作成](../../tutorials/ui/create/cloud-storage/azure-file-storage.md)
- [UIでのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/batch/cloud-storage.md)
