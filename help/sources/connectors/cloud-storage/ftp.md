---
keywords: Experience Platform；ホーム；人気のあるトピック；FTP;ftp;
solution: Experience Platform
title: FTPソースコネクタの概要
topic-legacy: overview
description: APIまたはユーザーインターフェイスを使用してFTPサーバーをAdobe Experience Platformに接続する方法を説明します。
exl-id: a6186fad-8a7b-4103-80c7-a522ff69fe9e
source-git-commit: 1f9948d6e419ee5d6a021a589378f7aa990b7291
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 2%

---

# （ベータ版）FTPコネクタ

>[!NOTE]
>
>FTPコネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ソースの概要](../../home.md#terms-and-conditions)」を参照してください。

Adobe Experience Platformは、AWS、[!DNL Google Cloud Platform]、[!DNL Azure]などのクラウドプロバイダーにネイティブ接続を提供し、これらのシステムからデータを取り込むことができます。

クラウドストレージソースは、ダウンロード、フォーマット、アップロードを必要とせずに、独自のデータを[!DNL Platform]に取り込むことができます。 取り込んだデータは、XDM JSON形式、XDM Parquet形式または区切り形式で指定できます。 プロセスの各手順は、ソースワークフローに統合されます。 [!DNL Platform] では、FTPまたはSFTPサーバーからデータをバッチで取り込むことができます。

>[!IMPORTANT]
>
>FTPソースコネクタを使用してデータフローを作成する場合、FTPサーバー内で発生する増分更新の問題が残るので、1回限りのインジェストスケジュールにを設定することを強くお勧めします。

## IPアドレス許可リスト

ソースコネクタを操作する前に、IPアドレスのリストを許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加しないと、ソースを使用する際にエラーやパフォーマンスが低下する可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)のページを参照してください。

## ファイルとディレクトリの命名の制約

以下は、クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストです。

- ディレクトリ名とファイルコンポーネント名は255文字以内にする必要があります。
- ディレクトリ名とファイル名の末尾にスラッシュ(`/`)を付けることはできません。 指定した場合、自動的に削除されます。
- 次の予約URL文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`.
- 無効なURLパス文字は使用できません。 `\uE000`のようなコードポイントは、NTFSファイル名では有効ですが、有効なUnicode文字ではありません。 また、制御文字（0x00～0x1F、\u0081など）のようなASCII文字やUnicode文字も使用できません。 HTTP/1.1でUnicode文字列を規定するルールについては、[RFC 2616、2.2節を参照してください。基本規則](https://www.ietf.org/rfc/rfc2616.txt)と[RFC 3987](https://www.ietf.org/rfc/rfc3987.txt)。
- 次のファイル名は使用できません。LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、prn、AUX、NUL、CON、CLOCK$、ドット文字(.)、2つのドット文字(..)。

## [!DNL Platform]にFTPを接続

以下のドキュメントでは、APIまたはユーザーインターフェイスを使用してFTPサーバーを[!DNL Platform]に接続する方法について説明します。

### APIの使用

- [フローサービスAPIを使用したFTPベース接続の作成](../../tutorials/api/create/cloud-storage/ftp.md)
- [フローサービスAPIを使用したクラウドストレージソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/cloud-storage.md)
- [フローサービスAPIを使用したクラウドストレージソースのデータフローの作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UIでのFTPソース接続の作成](../../tutorials/ui/create/cloud-storage/ftp.md)
- [UIでのクラウドストレージ接続のデータフローの作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)
