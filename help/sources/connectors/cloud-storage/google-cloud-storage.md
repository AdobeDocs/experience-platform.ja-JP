---
keywords: Experience Platform；ホーム；人気のあるトピック；Google Cloudストレージ;Googleクラウドストレージ
solution: Experience Platform
title: Google Cloudストレージソースコネクタの概要
topic-legacy: overview
description: APIまたはユーザーインターフェイスを使用してGoogle CloudストレージをAdobe Experience Platformに接続する方法を説明します。
exl-id: f7ebd213-f914-4c49-aebd-1df4514ffec0
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '541'
ht-degree: 5%

---

# Google Cloudストレージコネクタ

Adobe Experience Platformは、AWS、[!DNL Google Cloud Platform]、[!DNL Azure]などのクラウドプロバイダーにネイティブの接続を提供し、これらのシステムからデータを取り込むことができます。

クラウドストレージソースを使用すると、ダウンロード、フォーマット、アップロードをおこなう必要なく、独自のデータを Platform に取り込むことができます。取り込んだデータは、エクスペリエンスデータモデル(XDM)に準拠したJSONまたはパーケットとして、または区切り形式で形式設定できます。 プロセスのすべての手順がソースワークフローに統合されます。 プラットフォームでは、[!DNL Google Cloud Storage]からバッチを通じてデータを取り込むことができます。

## IPアドレス許可リスト

IPアドレスのリストは、ソースコネクタを使用する前に許可リストに追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。 詳しくは、[IPアドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## [!DNL Google Cloud Storage]アカウントを接続するための前提条件の設定

プラットフォームに接続するには、まず[!DNL Google Cloud Storage]アカウントの相互運用性を有効にする必要があります。 相互運用性設定にアクセスするには、[!DNL Google Cloud Platform]を開き、ナビゲーションパネルの&#x200B;**[!UICONTROL クラウドストレージ]**&#x200B;オプションから&#x200B;**[!UICONTROL 設定]**&#x200B;を選択します。

![](../../images/tutorials/create/google-cloud-storage/nav.png)

**[!UICONTROL 設定]**&#x200B;ページが表示されます。 [!DNL Google]プロジェクトIDに関する情報と[!DNL Google Cloud Storage]アカウントの詳細は、ここから確認できます。 相互運用性設定にアクセスするには、上部のヘッダーで&#x200B;**[!UICONTROL Interoperability]**&#x200B;を選択します。

![](../../images/tutorials/create/google-cloud-storage/project-access.png)

**[!UICONTROL 相互運用性]**&#x200B;ページには、認証、アクセスキー、およびサービスアカウントに関連付けられたデフォルトのプロジェクトに関する情報が含まれています。 サービスアカウントの新しいアクセスキーIDとシークレットアクセスキーを生成するには、「**[!UICONTROL サービスアカウントのキーを作成]**」を選択します。

![](../../images/tutorials/create/google-cloud-storage/interoperability.png)

新しく生成したアクセスキーIDとシークレットアクセスキーを使用して、[!DNL Google Cloud Storage]アカウントをプラットフォームに接続できます。

## ファイルとディレクトリの命名規則

次に、クラウドストレージのファイルやディレクトリに名前を付ける際に考慮する必要がある制約のリストを示します。

- ディレクトリ名とファイルコンポーネント名は255文字以内にする必要があります。
- ディレクトリ名とファイル名の末尾にスラッシュ(`/`)を付けることはできません。 指定した場合は、自動的に削除されます。
- 次の予約済みURL文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`.
- 無効なURLパス文字は使用できません。 `\uE000`のようなコードポイントは、NTFSファイル名では有効ですが、有効なUnicode文字ではありません。 また、制御文字（0x00 ～ 0x1F、\u0081など）のようなASCII文字やUnicode文字も使用できません。 HTTP/1.1でUnicode文字列を扱うルールについては、[RFC 2616, Section 2.2を参照してください。基本規則](https://www.ietf.org/rfc/rfc2616.txt)と[RFC 3987](https://www.ietf.org/rfc/rfc3987.txt)。
- 次のファイル名は使用できません。LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、prn、AUX、NUL、CON、CLOCK$、ドット文字(.)および2つのドット文字(..)。

## [!DNL Google Cloud Storage]をプラットフォームに接続

以下のドキュメントは、APIまたはユーザーインターフェイスを使用して[!DNL Google Cloud Storage]をプラットフォームに接続する方法に関する情報を提供しています。

### APIの使用

- [Flow Service APIを使用してGoogle Cloudストレージソース接続を作成する](../../tutorials/api/create/cloud-storage/google.md)
- [Flow Service APIを使用したクラウドストレージシステムの調査](../../tutorials/api/explore/cloud-storage.md)
- [Flow Service APIを使用してクラウドストレージデータを収集する](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UIでのGoogle Cloudストレージソース接続の作成](../../tutorials/ui/create/cloud-storage/google-cloud-storage.md)
- [UIでのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/batch/cloud-storage.md)
