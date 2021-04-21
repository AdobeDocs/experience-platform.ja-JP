---
keywords: Experience Platform；ホーム；人気の高いトピック；SFTP;SFTP
solution: Experience Platform
title: SFTPソースコネクタの概要
topic-legacy: overview
description: APIまたはユーザーインターフェイスを使用してSFTPサーバーをAdobe Experience Platformに接続する方法を説明します。
exl-id: d5bced3d-cd33-40ea-bce0-32c76ecd2790
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 2%

---

# SFTPコネクタ

Adobe Experience Platformは、AWS、[!DNL Google Cloud Platform]、[!DNL Azure]などのクラウドプロバイダーにネイティブの接続を提供し、これらのシステムからデータを取り込むことができます。

Cloudストレージソースは、ダウンロード、フォーマット、アップロードを必要とせずに、独自のデータを[!DNL Platform]に取り込むことができます。 取り込んだデータは、XDM JSON、XDM Parket、または区切り形式で形式設定できます。 プロセスの各手順は、Sourcesワークフローに統合されます。 [!DNL Platform] FTPまたはSFTPサーバーからデータをバッチで取り込むことができます。

## IPアドレス許可リスト

IPアドレスのリストは、ソースコネクタを使用する前に許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## ファイルとディレクトリの命名規則

次に、クラウドストレージのファイルやディレクトリに名前を付ける際に考慮する必要がある制約のリストを示します。

- ディレクトリ名とファイルコンポーネント名は255文字以内にする必要があります。
- ディレクトリ名とファイル名の末尾にスラッシュ(`/`)を付けることはできません。 指定した場合は、自動的に削除されます。
- 次の予約済みURL文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`.
- 無効なURLパス文字は使用できません。 `\uE000`のようなコードポイントは、NTFSファイル名では有効ですが、有効なUnicode文字ではありません。 また、制御文字（0x00 ～ 0x1F、\u0081など）のようなASCII文字やUnicode文字も使用できません。 HTTP/1.1でUnicode文字列を扱うルールについては、[RFC 2616, Section 2.2を参照してください。基本規則](https://www.ietf.org/rfc/rfc2616.txt)と[RFC 3987](https://www.ietf.org/rfc/rfc3987.txt)。
- 次のファイル名は使用できません。LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、prn、AUX、NUL、CON、CLOCK$、ドット文字(.)および2つのドット文字(..)。

## SFTPを[!DNL Platform]に接続

>[!IMPORTANT]
>
>ユーザーは、接続前にSFTPサーバー設定でキーボードインタラクティブ認証を無効にする必要があります。 この設定を無効にすると、サービスやプログラムを通じて入力するのではなく、手動でパスワードを入力できます。 Keyboard Interactive Authenticationについて詳しくは、[Component Proドキュメント](https://doc.componentpro.com/ComponentPro-Sftp/authenticating-with-a-keyboard-interactive-authentication)を参照してください。

以下のドキュメントは、APIまたはユーザーインターフェイスを使用してSFTPサーバーを[!DNL Platform]に接続する方法に関する情報を提供しています。

### APIの使用

- [Flow Service APIを使用したSFTPソース接続の作成](../../tutorials/api/create/cloud-storage/sftp.md)
- [Flow Service APIを使用したクラウドストレージシステムの調査](../../tutorials/api/explore/cloud-storage.md)
- [Flow Service APIを使用してクラウドストレージデータを収集する](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UIでのSFTPソース接続の作成](../../tutorials/ui/create/cloud-storage/sftp.md)
- [UIでのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/batch/cloud-storage.md)
