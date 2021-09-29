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

[!DNL Data Landing Zone] は、Adobe Experience Platformによっ [!DNL Azure Blob] てプロビジョニングされたストレージインターフェイスで、ソースや宛先を介してPlatformの内外にファイルを取り込み、出し入れする、セキュアなクラウドベースのファイルストレージ機能にアクセスできます。サンドボックスごとに1つの[!DNL Data Landing Zone]コンテナにアクセスでき、すべてのコンテナにわたる合計データ量は、Platform Products and Servicesライセンスで提供される合計データ量に制限されます。 [!DNL Customer Journey Analytics]、[!DNL Journey Orchestration]、[!DNL Intelligent Services]、[!DNL Real-time Customer Data Platform]など、Platformとそのアプリケーションサービスのすべてのお客様は、サンドボックスごとに1つの[!DNL Data Landing Zone]コンテナを使用してプロビジョニングされます。 [!DNL Azure Storage Explorer]またはコマンドラインインターフェイスを使用して、コンテナにファイルを読み書きできます。

[!DNL Data Landing Zone] は、SASベースの認証をサポートし、そのデータは、保存時と送信時の標準のス [!DNL Azure Blob] トレージセキュリティメカニズムで保護されています。SASベースの認証を使用すると、パブリックインターネット接続を介して[!DNL Data Landing Zone]コンテナに安全にアクセスできます。 [!DNL Data Landing Zone]コンテナにアクセスするために必要なネットワークの変更はありません。つまり、ネットワークの許可リストやクロスリージョンの設定を構成する必要はありません。 Platformでは、[!DNL Data Landing Zone]コンテナにアップロードされたすべてのファイルに対して、厳密な7日間の有効期間(TTL)が適用されます。 すべてのファイルは7日後に削除されます。

## ファイルとディレクトリの命名の制約

以下は、クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストです。

- ディレクトリ名とファイルコンポーネント名は255文字以内にする必要があります。
- ディレクトリ名とファイル名の末尾にスラッシュ(`/`)を付けることはできません。 指定した場合、自動的に削除されます。
- 次の予約URL文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`.
- 無効なURLパス文字は使用できません。 `\uE000`のようなコードポイントは、NTFSファイル名では有効ですが、有効なUnicode文字ではありません。 また、制御文字（`0x00`から`0x1F`、`\u0081`など）のような、ASCII文字やUnicode文字も使用できません。 HTTP/1.1でUnicode文字列を規定するルールについては、[RFC 2616、2.2節を参照してください。基本規則](https://www.ietf.org/rfc/rfc2616.txt)と[RFC 3987](https://www.ietf.org/rfc/rfc3987.txt)。
- 次のファイル名は使用できません。LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、prn、AUX、NUL、CON、CLOCK$、ドット文字(.)、2つのドット文字(..)。

## [!DNL Data Landing Zone]を[!DNL Platform]に接続します

以下のドキュメントでは、APIまたはユーザーインターフェイスを使用して[!DNL Data Landing Zone]コンテナからAdobe Experience Platformにデータを取り込む方法について説明します。

### API の使用

- [フローサービスAPIを使用して [!DNL Data Landing Zone] ソース接続を作成する](../../tutorials/api/create/cloud-storage/data-landing-zone.md)
- [フローサービスAPIを使用したクラウドストレージソースのデータフローの作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UIを使用してプラットフォームに接続する [!DNL Data Landing Zone] ](../../tutorials/ui/create/cloud-storage/data-landing-zone.md)
- [UIでのクラウドストレージ接続のデータフローの作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)
