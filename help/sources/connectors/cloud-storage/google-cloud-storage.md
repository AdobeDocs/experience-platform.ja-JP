---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Google Cloudストレージコネクタ
topic: overview
translation-type: tm+mt
source-git-commit: 340f5d0611e9e9eb4676018ee10c8a8aa08dbb2d
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 3%

---


# Google Cloudストレージコネクタ

Adobe Experience Platformは、AWS、 [!DNL Google Cloud Platform]およびなどのクラウドプロバイダーに対してネイティブの接続を提供し、これらのシステムからデータを取得でき [!DNL Azure]ます。

Cloud storage sources can bring your own data into [!DNL Platform] without the need to download, format, or upload. 取り込んだデータは、XDM JSON、XDMパーケー、または区切り文字として形式設定できます。 プロセスの各手順は、Sourcesワークフローに統合されます。 [!DNL Platform] バッチを使用してデータを取り込むこ [!DNL Google Cloud Storage] とができます。

## IPアドレス許可リスト

ソースコネクタを使用する前に、許可リストに次のIPアドレスを追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。

### 米国東部地域

- `20.41.2.0/23`
- `20.41.4.0/26`
- `20.44.17.80/28`
- `20.49.102.16/29`
- `40.70.148.160/28`
- `52.167.107.224/28`

### 西ヨーロッパ地域

- `13.69.67.192/28`
- `13.69.107.112/28`
- `13.69.112.128/28`
- `40.74.24.192/26`
- `40.74.26.0/23`
- `40.113.176.232/29`
- `52.236.187.112/28`

### オーストラリア東部

- `13.70.74.144/28`
- `20.37.193.0/25`
- `20.37.193.128/26`
- `20.37.198.224/29`
- `40.79.163.80/28`
- `40.79.171.160/28`

## アカウント接続の前提条件の [!DNL Google Cloud Storage] 設定

に接続するに [!DNL Platform]は、最初にアカウントの相互運用性を有効にする必要があり [!DNL Google Cloud Storage] ます。 相互運用性設定にアクセスするには、を開き、ナビゲーションパネルの [!DNL Google Cloud Platform] ストレージ **[!UICONTROL (]** Settings **[!UICONTROL )オプションから「]** Settings」を選択します。

![](../../images/tutorials/create/google-cloud-storage/nav.png)

The **[!UICONTROL Settings]** page appears. ここから、 [!DNL Google] プロジェクトIDに関する情報とアカウントに関する詳細を確認でき [!DNL Google Cloud Storage] ます。 相互運用性設定にアクセスするには、上部のヘッダーで **[!UICONTROL 「Interoperability]** 」を選択します。

![](../../images/tutorials/create/google-cloud-storage/project-access.png)

[ **[!UICONTROL 相互運用性]** ]ページには、認証、アクセスキー、およびユーザアカウントに関連付けられた既定のプロジェクトに関する情報が含まれます。 相互運用可能アクセス用の既定のプロジェクトをまだ確立していない場合は、[相互運用可能アクセス用の *[!UICONTROL 既定のプロジェクト]* ]セクションで設定できます。 既定のプロジェクトが既に確立されている場合は、このセクションに、プロジェクトが既定として設定されていることを確認するメッセージが表示されます。

ユーザーアカウントの新しいアクセスキーIDと秘密鍵を生成するには、「キーを **[!UICONTROL 作成]**」を選択します。

![](../../images/tutorials/create/google-cloud-storage/interoperability.png)

新しく生成したアクセスキーIDと秘密鍵を使用して、 [!DNL Google Cloud Storage] アカウントをに接続でき [!DNL Platform]ます。

## 接続 [!DNL Google Cloud Storage] 先 [!DNL Platform]

次のドキュメントは、APIまたはユーザーインターフェイス [!DNL Google Cloud Storage] を [!DNL Platform] 使用した接続方法に関する情報を提供しています。

### APIの使用

- [Flow Service APIを使用してGoogle Cloudストレージコネクタを作成する](../../tutorials/api/create/cloud-storage/google.md)
- [Flow Service APIを使用したクラウドストレージシステムの調査](../../tutorials/api/explore/cloud-storage.md)
- [Flow Service APIを使用してクラウドストレージデータを収集する](../../tutorials/api/collect/cloud-storage.md)

### UI の使用

- [UI での Google Cloud ストレージソースコネクタの作成](../../tutorials/ui/create/cloud-storage/google-cloud-storage.md)
- [UIでのクラウドストレージコネクタのデータフローの設定](../../tutorials/ui/dataflow/batch/cloud-storage.md)