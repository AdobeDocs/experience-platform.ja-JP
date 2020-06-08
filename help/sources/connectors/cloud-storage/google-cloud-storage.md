---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Google Cloudストレージコネクタ
topic: overview
translation-type: tm+mt
source-git-commit: 0ed2ed3b08f262100746f255a78c248a1748eb5e
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%

---


# Google Cloudストレージコネクタ

Adobe Experience Platformは、AWS、Google Cloud Platform、Azureなどのクラウドプロバイダーに対してネイティブの接続性を提供し、これらのシステムからデータを取り込むことができます。

クラウドストレージソースは、ダウンロード、形式設定、アップロードを行うことなく、独自のデータをプラットフォームに取り込むことができます。 取り込んだデータは、XDM JSON、XDMパーケー、または区切り文字として形式設定できます。 プロセスの各手順は、Sourcesワークフローに統合されます。 プラットフォームを使用すると、Google Cloudストレージからデータをバッチで取り込むことができます。

## Google Cloudストレージアカウント接続の前提条件の設定

Platformに接続するには、まずGoogle Cloudストレージアカウントの相互運用を有効にする必要があります。 相互運用性設定にアクセスするには、Google Cloud Platformを開き、ナビゲーションパネルの **[!UICONTROL ストレージ]****[!UICONTROL (Settings]** )オプションを選択します。

![](../../images/tutorials/create/google-cloud-storage/nav.png)

[ **[!UICONTROL 設定]** ]ページが表示されます。 ここから、GoogleプロジェクトIDに関する情報とGoogle Cloudストレージアカウントに関する詳細を確認できます。 相互運用性設定にアクセスするには、上部のヘッダーで **[!UICONTROL 「Interoperability]** 」を選択します。

![](../../images/tutorials/create/google-cloud-storage/project-access.png)

[ **[!UICONTROL 相互運用性]** ]ページには、認証、アクセスキー、およびユーザアカウントに関連付けられた既定のプロジェクトに関する情報が含まれます。 相互運用可能アクセス用の既定のプロジェクトをまだ確立していない場合は、[相互運用可能アクセス用の *[!UICONTROL 既定のプロジェクト]* ]セクションで設定できます。 既定のプロジェクトが既に確立されている場合は、このセクションに、プロジェクトが既定として設定されていることを確認するメッセージが表示されます。

ユーザーアカウントの新しいアクセスキーIDと秘密鍵を生成するには、「キーを **[!UICONTROL 作成]**」を選択します。

![](../../images/tutorials/create/google-cloud-storage/interoperability.png)

新しく生成したアクセスキーIDと秘密アクセスキーを使用して、Google Cloudストレージアカウントをプラットフォームに接続できます。

以下のドキュメントは、APIまたはユーザーインターフェイスを使用してGoogle Cloudストレージをプラットフォームに接続する方法に関する情報を提供しています。

## Google Cloudストレージのプラットフォームへの接続

以下のドキュメントは、APIまたはユーザーインターフェイスを使用してGoogle Cloudストレージをプラットフォームに接続する方法に関する情報を提供しています。

### APIの使用

- [Flow Service APIを使用してGoogle Cloudストレージコネクタを作成する](../../tutorials/api/create/cloud-storage/google.md)
- [Flow Service APIを使用したクラウドストレージシステムの調査](../../tutorials/api/explore/cloud-storage.md)
- [Flow Service APIを使用してクラウドストレージデータを収集する](../../tutorials/api/collect/cloud-storage.md)

### UIの使用

- [UIでのGoogle Cloudストレージソースコネクタの作成](../../tutorials/ui/create/cloud-storage/google-cloud-storage.md)
- [UIでのクラウドストレージコネクタのデータフローの設定](../../tutorials/ui/dataflow/batch/cloud-storage.md)