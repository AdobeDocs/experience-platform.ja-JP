---
keywords: Experience Platform；ホーム；人気のあるトピック；Google Cloud Storage;googleクラウドストレージ
solution: Experience Platform
title: Google Cloud Storage Sourceコネクタの概要
topic-legacy: overview
description: APIまたはユーザーインターフェイスを使用してGoogle Cloud StorageをAdobe Experience Platformに接続する方法を説明します。
exl-id: f7ebd213-f914-4c49-aebd-1df4514ffec0
source-git-commit: 1f9948d6e419ee5d6a021a589378f7aa990b7291
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 5%

---

# Google Cloud Storageコネクタ

Adobe Experience Platformは、AWS、[!DNL Google Cloud Platform]、[!DNL Azure]などのクラウドプロバイダーにネイティブ接続を提供し、これらのシステムからデータを取り込むことができます。

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードをおこなう必要なく、独自のデータを Platform に取り込むことができます。取り込むデータは、エクスペリエンスデータモデル(XDM)に準拠したJSONまたはParquet形式または区切り形式で形式設定できます。 プロセスの各ステップは、ソースワークフローに統合されます。 Platformでは、[!DNL Google Cloud Storage]からデータをバッチで取り込むことができます。

## IPアドレス許可リスト

ソースコネクタを操作する前に、IPアドレスのリストを許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加しないと、ソースを使用する際にエラーやパフォーマンスが低下する可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)のページを参照してください。

## [!DNL Google Cloud Storage]アカウントを接続するための前提条件の設定

Platformに接続するには、まず[!DNL Google Cloud Storage]アカウントの相互運用性を有効にする必要があります。 相互運用性の設定にアクセスするには、[!DNL Google Cloud Platform]を開き、ナビゲーションパネルの「**[!UICONTROL クラウドストレージ]**」オプションから「**[!UICONTROL 設定]**」を選択します。

![](../../images/tutorials/create/google-cloud-storage/nav.png)

**[!UICONTROL 設定]**&#x200B;ページが表示されます。 ここから、[!DNL Google]プロジェクトIDに関する情報と[!DNL Google Cloud Storage]アカウントの詳細を確認できます。 相互運用性の設定にアクセスするには、上部のヘッダーから「**[!UICONTROL Interoperability]**」を選択します。

![](../../images/tutorials/create/google-cloud-storage/project-access.png)

**[!UICONTROL Interoperability]**&#x200B;ページには、認証、アクセスキー、およびサービスアカウントに関連付けられたデフォルトプロジェクトに関する情報が含まれています。 新しいアクセスキーIDとサービスアカウントの秘密アクセスキーを生成するには、「**[!UICONTROL サービスアカウントのキーを作成]**」を選択します。

![](../../images/tutorials/create/google-cloud-storage/interoperability.png)

新しく生成されたアクセスキーIDと秘密アクセスキーを使用して、[!DNL Google Cloud Storage]アカウントをPlatformに接続できます。

## ファイルとディレクトリの命名の制約

以下は、クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストです。

- ディレクトリ名とファイルコンポーネント名は255文字以内にする必要があります。
- ディレクトリ名とファイル名の末尾にスラッシュ(`/`)を付けることはできません。 指定した場合、自動的に削除されます。
- 次の予約URL文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`.
- 無効なURLパス文字は使用できません。 `\uE000`のようなコードポイントは、NTFSファイル名では有効ですが、有効なUnicode文字ではありません。 また、制御文字（0x00～0x1F、\u0081など）のようなASCII文字やUnicode文字も使用できません。 HTTP/1.1でUnicode文字列を規定するルールについては、[RFC 2616、2.2節を参照してください。基本規則](https://www.ietf.org/rfc/rfc2616.txt)と[RFC 3987](https://www.ietf.org/rfc/rfc3987.txt)。
- 次のファイル名は使用できません。LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、prn、AUX、NUL、CON、CLOCK$、ドット文字(.)、2つのドット文字(..)。

## [!DNL Google Cloud Storage]をPlatformに接続

以下のドキュメントでは、APIまたはユーザーインターフェイスを使用して[!DNL Google Cloud Storage]をPlatformに接続する方法について説明します。

### API の使用

- [フローサービスAPIを使用したGoogle Cloud Storageベースの接続の作成](../../tutorials/api/create/cloud-storage/google.md)
- [フローサービスAPIを使用したクラウドストレージソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/cloud-storage.md)
- [フローサービスAPIを使用したクラウドストレージソースのデータフローの作成](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UIでのGoogle Cloud Storageソース接続の作成](../../tutorials/ui/create/cloud-storage/google-cloud-storage.md)
- [UIでのクラウドストレージ接続のデータフローの作成](../../tutorials/ui/dataflow/batch/cloud-storage.md)
