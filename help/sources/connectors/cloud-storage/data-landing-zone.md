---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: データランディングゾーンのソース
topic-legacy: overview
description: データランディングゾーンをAdobe Experience Platformに接続する方法を説明します
exl-id: bdc10095-7de4-4183-bfad-a7b5c89197e3
source-git-commit: ca7197036283ee15dbf60c113d361a5ea34d65c1
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 4%

---

# [!DNL Data Landing Zone]

[!DNL Data Landing Zone] は、Adobe Experience Platformによっ [!DNL Azure Blob] てプロビジョニングされたストレージインターフェイスで、ソースや宛先を介して Platform の内外でファイルを取り込み、出し入れする、安全なクラウドベースのファイルストレージ機能にアクセスできます。サンドボックスごとに 1 つの [!DNL Data Landing Zone] コンテナにアクセスでき、すべてのコンテナの合計データ量は、Platform Products and Services ライセンスで提供される合計データ量に制限されます。 [!DNL Customer Journey Analytics]、[!DNL Journey Orchestration]、[!DNL Intelligent Services]、[!DNL Real-time Customer Data Platform] など、Platform とそのアプリケーションサービスのすべてのお客様は、サンドボックスごとに 1 つの [!DNL Data Landing Zone] コンテナを使用してプロビジョニングされます。 [!DNL Azure Storage Explorer] またはコマンドラインインターフェイスを使用して、コンテナにファイルを読み書きできます。

[!DNL Data Landing Zone] は SAS ベースの認証をサポートし、そのデータは標準のストレージセキュリテ [!DNL Azure Blob] ィメカニズムで保護され、保存時と送信時に使用されます。SAS ベースの認証を使用すると、[!DNL Data Landing Zone] コンテナにパブリックインターネット接続を介して安全にアクセスできます。 [!DNL Data Landing Zone] コンテナにアクセスするためにネットワークの変更は必要ありません。つまり、ネットワークの許可リストやクロスリージョンの設定を構成する必要はありません。 Platform では、[!DNL Data Landing Zone] コンテナにアップロードされたすべてのファイルに対して、厳密な 7 日間の有効期間 (TTL) が適用されます。 すべてのファイルは 7 日後に削除されます。

## ファイルとディレクトリの命名制約

以下は、クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストです。

- ディレクトリとファイルコンポーネントの名前は 255 文字以下にする必要があります。
- ディレクトリ名とファイル名の末尾にスラッシュ (`/`) を付けることはできません。 指定した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`.
- 無効な URL パス文字は使用できません。 `\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。 また、制御文字（`0x00` ～ `0x1F`、`\u0081` など）など、ASCII 文字や Unicode 文字の一部も使用できません。 HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、Section 2.2 を参照してください。基本規則 ](https://www.ietf.org/rfc/rfc2616.txt) と [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt)。
- 次のファイル名は使用できません。LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, LPT9, COM1, COM2, COM3, COM4, COM5, COM6, COM8, COM9,prn、AUX、NUL、CON、CLOCK$、ドット文字 (.)、2 つのドット文字 (..)。

## [!DNL Data Landing Zone] を [!DNL Platform] に接続

以下のドキュメントでは、API またはユーザーインターフェイスを使用して [!DNL Data Landing Zone] コンテナからAdobe Experience Platformにデータを取り込む方法について説明します。

### API の使用

- [フローサービス API を使用して  [!DNL Data Landing Zone]  ソース接続を作成する](../../tutorials/api/create/cloud-storage/data-landing-zone.md)
- [フローサービス API を使用したクラウドストレージソースのデータフローの作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UI を使用して Platform に接続します。 [!DNL Data Landing Zone] ](../../tutorials/ui/create/cloud-storage/data-landing-zone.md)
- [UI でのクラウドストレージ接続のデータフローの作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)
