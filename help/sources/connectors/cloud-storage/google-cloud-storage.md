---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Google Cloudストレージコネクタ
topic: overview
translation-type: tm+mt
source-git-commit: 6ffdcc2143914e2ab41843a52dc92344ad51bcfb
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---


# Google Cloudストレージコネクタ

Adobe Experience Platformは、AWS、 [!DNL Google Cloud Platform]およびなどのクラウドプロバイダーに対してネイティブの接続を提供し、これらのシステムからデータを取得でき [!DNL Azure]ます。

Cloudストレージソースからは、ダウンロード、フォーマット、アップロードを行う [!DNL Platform] ことなく、独自のデータをに取り込むことができます。 取り込んだデータは、XDM JSON、XDMパーケー、または区切り文字として形式設定できます。 プロセスの各手順は、Sourcesワークフローに統合されます。 [!DNL Platform] バッチを使用してデータを取り込むこ [!DNL Google Cloud Storage] とができます。

## アカウント接続の前提条件の [!DNL Google Cloud Storage] 設定

に接続するに [!DNL Platform]は、最初にアカウントの相互運用性を有効にする必要があり [!DNL Google Cloud Storage] ます。 相互運用性設定にアクセスするには、を開き、ナビゲーションパネルの [!DNL Google Cloud Platform] ストレージ **[!UICONTROL (]** Settings **[!UICONTROL )オプションから「]** Settings」を選択します。

![](../../images/tutorials/create/google-cloud-storage/nav.png)

[ **[!UICONTROL 設定]** ]ページが表示されます。 ここから、 [!DNL Google] プロジェクトIDに関する情報とアカウントに関する詳細を確認でき [!DNL Google Cloud Storage] ます。 相互運用性設定にアクセスするには、上部のヘッダーで **[!UICONTROL 「Interoperability]** 」を選択します。

![](../../images/tutorials/create/google-cloud-storage/project-access.png)

[ **[!UICONTROL 相互運用性]** ]ページには、認証、アクセスキー、およびユーザアカウントに関連付けられた既定のプロジェクトに関する情報が含まれます。 相互運用可能アクセス用の既定のプロジェクトをまだ確立していない場合は、[相互運用可能アクセス用の *[!UICONTROL 既定のプロジェクト]* ]セクションで設定できます。 既定のプロジェクトが既に確立されている場合は、このセクションに、プロジェクトが既定として設定されていることを確認するメッセージが表示されます。

ユーザーアカウントの新しいアクセスキーIDと秘密鍵を生成するには、「キーを **[!UICONTROL 作成]**」を選択します。

![](../../images/tutorials/create/google-cloud-storage/interoperability.png)

新しく生成したアクセスキーIDと秘密鍵を使用して、 [!DNL Google Cloud Storage] アカウントをに接続でき [!DNL Platform]ます。

次のドキュメントは、APIまたはユーザーインターフェイス [!DNL Google Cloud Storage] を [!DNL Platform] 使用した接続方法に関する情報を提供しています。

## 接続 [!DNL Google Cloud Storage] 先 [!DNL Platform]

次のドキュメントは、APIまたはユーザーインターフェイス [!DNL Google Cloud Storage] を [!DNL Platform] 使用した接続方法に関する情報を提供しています。

### APIの使用

- [Flow Service APIを使用してGoogle Cloudストレージコネクタを作成する](../../tutorials/api/create/cloud-storage/google.md)
- [Flow Service APIを使用したクラウドストレージシステムの調査](../../tutorials/api/explore/cloud-storage.md)
- [Flow Service APIを使用してクラウドストレージデータを収集する](../../tutorials/api/collect/cloud-storage.md)

### UIの使用

- [UIでのGoogle Cloudストレージソースコネクタの作成](../../tutorials/ui/create/cloud-storage/google-cloud-storage.md)
- [UIでのクラウドストレージコネクタのデータフローの設定](../../tutorials/ui/dataflow/batch/cloud-storage.md)