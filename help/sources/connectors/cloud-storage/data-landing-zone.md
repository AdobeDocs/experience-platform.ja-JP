---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: データランディングゾーンのソース
topic-legacy: overview
description: データランディングゾーンをAdobe Experience Platformに接続する方法を説明します
exl-id: bdc10095-7de4-4183-bfad-a7b5c89197e3
source-git-commit: ecc9bc603bfd7b56f5f232b0d6d91eb65a901510
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 4%

---

# [!DNL Data Landing Zone]

[!DNL Data Landing Zone] は [!DNL Azure Blob] Adobe Experience Platformによってプロビジョニングされたストレージインターフェイス。ファイルを Platform に取り込むための、セキュリティで保護されたクラウドベースのファイルストレージ機能にアクセスできます。 アクセスできるユーザー [!DNL Data Landing Zone] サンドボックスごとのコンテナと、すべてのコンテナの合計データ量は、Platform 製品およびサービスライセンスで提供される合計データに制限されます。 Platform とそのアプリケーションサービスのすべてのお客様 ( [!DNL Customer Journey Analytics], [!DNL Journey Orchestration], [!DNL Intelligent Services]および [!DNL Real-time Customer Data Platform] プロビジョニングが [!DNL Data Landing Zone] コンテナ（サンドボックスごと） を使用して、コンテナにファイルを読み取り、書き込むことができます。 [!DNL Azure Storage Explorer] またはコマンドラインインターフェイスを使用します。

[!DNL Data Landing Zone] は SAS ベースの認証をサポートし、そのデータは標準で保護されています [!DNL Azure Blob] 保存時および送信時のストレージセキュリティメカニズム SAS ベースの認証を使用すると、 [!DNL Data Landing Zone] コンテナを使用して、公開インターネット接続を設定できます。 ユーザーが [!DNL Data Landing Zone] コンテナを使用する場合は、ネットワーク用に許可リストやクロスリージョンの設定を設定する必要はありません。 Platform では、 [!DNL Data Landing Zone] コンテナ。 すべてのファイルは 7 日後に削除されます。

## ファイルとディレクトリの命名制約

以下は、クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストです。

- ディレクトリとファイルコンポーネントの名前は 255 文字以下にする必要があります。
- ディレクトリ名とファイル名の末尾にスラッシュ (`/`) をクリックします。 指定した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。 `! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。 `" \ / : | < > * ?`.
- 無効な URL パス文字は使用できません。 次のようなコードポイント `\uE000`は NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。 また、制御文字 ( `0x00` を `0x1F`, `\u0081`など ) は使用できません。 HTTP/1.1 での Unicode 文字列を規定するルールについては、 [RFC 2616、セクション 2.2:基本ルール](https://www.ietf.org/rfc/rfc2616.txt) および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt).
- 次のファイル名は使用できません。LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, LPT9, COM1, COM2, COM3, COM4, COM5, COM6, COM8, COM9,prn、AUX、NUL、CON、CLOCK$、ドット文字 (.)、2 つのドット文字 (..)。

## 接続 [!DNL Data Landing Zone] を [!DNL Platform]

以下のドキュメントでは、 [!DNL Data Landing Zone] API またはユーザーインターフェイスを使用してAdobe Experience Platformにコンテナを追加できます。

### API の使用

- [の作成 [!DNL Data Landing Zone] フローサービス API を使用したソース接続](../../tutorials/api/create/cloud-storage/data-landing-zone.md)
- [フローサービス API を使用したクラウドストレージソースのデータフローの作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [接続 [!DNL Data Landing Zone] UI を使用してプラットフォームを作成するには](../../tutorials/ui/create/cloud-storage/data-landing-zone.md)
- [UI でのクラウドストレージ接続のデータフローの作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)
