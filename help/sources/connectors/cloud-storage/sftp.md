---
keywords: Experience Platform；ホーム；人気のあるトピック；SFTP;sftp
solution: Experience Platform
title: SFTP ソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して SFTP サーバーをAdobe Experience Platformに接続する方法を説明します。
exl-id: d5bced3d-cd33-40ea-bce0-32c76ecd2790
source-git-commit: 1f9948d6e419ee5d6a021a589378f7aa990b7291
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 2%

---

# SFTP コネクタ

Adobe Experience Platformは、AWS、[!DNL Google Cloud Platform]、[!DNL Azure] などのクラウドプロバイダーにネイティブ接続を提供し、これらのシステムからデータを取り込むことができます。

クラウドストレージソースは、ダウンロード、フォーマット、アップロードを必要とせずに、独自のデータを [!DNL Platform] に取り込むことができます。 取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で指定できます。 プロセスの各ステップは、ソースワークフローに統合されます。 [!DNL Platform] では、FTP または SFTP サーバーからデータをバッチで取り込むことができます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する可能性があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md) ページを参照してください。

## ファイルとディレクトリの命名制約

以下は、クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストです。

- ディレクトリとファイルコンポーネントの名前は 255 文字以下にする必要があります。
- ディレクトリ名とファイル名の末尾にスラッシュ (`/`) を付けることはできません。 指定した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`.
- 無効な URL パス文字は使用できません。 `\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。 また、制御文字（0x00 ～ 0x1F、\u0081 など）など、ASCII 文字や Unicode 文字の一部も使用できません。 HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、Section 2.2 を参照してください。基本規則 ](https://www.ietf.org/rfc/rfc2616.txt) と [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt)。
- 次のファイル名は使用できません。LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, LPT9, COM1, COM2, COM3, COM4, COM5, COM6, COM8, COM9,prn、AUX、NUL、CON、CLOCK$、ドット文字 (.)、2 つのドット文字 (..)。

## SFTP を [!DNL Platform] に接続

>[!IMPORTANT]
>
>ユーザーは、接続前に SFTP サーバー設定でキーボードインタラクティブ認証を無効にする必要があります。 この設定を無効にすると、サービスやプログラムを通じて入力するのではなく、パスワードを手動で入力できます。 キーボードインタラクティブ認証の詳細については、[Component Pro のドキュメント ](https://doc.componentpro.com/ComponentPro-Sftp/authenticating-with-a-keyboard-interactive-authentication) を参照してください。

以下のドキュメントでは、API またはユーザーインターフェイスを使用して SFTP サーバーを [!DNL Platform] に接続する方法について説明します。

### API の使用

- [フローサービス API を使用した SFTP ベース接続の作成](../../tutorials/api/create/cloud-storage/sftp.md)
- [フローサービス API を使用したクラウドストレージソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/cloud-storage.md)
- [フローサービス API を使用したクラウドストレージソースのデータフローの作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UI での SFTP ソース接続の作成](../../tutorials/ui/create/cloud-storage/sftp.md)
- [UI でのクラウドストレージ接続のデータフローの作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)
