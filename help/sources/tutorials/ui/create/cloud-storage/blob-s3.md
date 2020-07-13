---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIにAzure BlobまたはAmazon S3ソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 3f1c3c77a0755a3e305da0fb8a234be0f0ee1863
workflow-type: tm+mt
source-wordcount: '631'
ht-degree: 1%

---


# UIで [!DNL Azure Blob] または [!DNL Amazon] S3ソースコネクタを作成する

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、ユー [!DNL Azure Blob] ザーインターフェイスを使用して、S3または [!DNL Amazon][!DNL Platform] S3 （以下「Blob」と呼びます）ソースコネクタを作成する手順について説明します。

## はじめに

このチュートリアルでは、次のAdobe Experience Platformのコンポーネントについて十分に理解している必要があります。

- [Experience Data Model(XDM)System](../../../../../xdm/home.md): Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成の主な原則とベストプラクティスが含まれます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既にBlobまたはS3ベースの接続がある場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/batch/cloud-storage.md)。

### サポートされているファイル形式

[!DNL Experience Platform] は、外部ストレージから取り込む次のファイル形式をサポートしています。

- 区切り文字区切り値(DSV): DSV形式のデータ・ファイルのサポートは、現在、コンマ区切り値に制限されています。 DSV形式のファイル内のフィールド・ヘッダーの値は、英数字とアンダースコアのみで構成する必要があります。 一般的なDSVファイルは、今後サポートされる予定です。
- JavaScript Object Notation (JSON): JSON形式のデータファイルは、XDMに準拠している必要があります。
- Apacheパーケット： パーケット形式のデータファイルは、XDMに準拠している必要があります。

### 必要な資格情報の収集

のBlobストレージにアクセスするに [!DNL Platform]は、次の資格情報に対して有効な値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | BLOBストレージのデータにアクセスするために必要な接続文字列です。 BLOB接続文字列パターンは次のとおりです。 `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`. |

開始方法の詳細については、 [このAzure Blobドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string)。

同様に、でS3バケットにアクセスするに [!DNL Platform] は、次の資格情報の有効値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `s3AccessKey` | S3ストレージのアクセスキーID。 |
| `s3SecretKey` | S3ストレージの秘密鍵ID。 |

開始方法の詳細については、 [このAWSドキュメントを参照してください](https://aws.amazon.com/blogs/security/wheres-my-secret-access-key/)。

## BlobまたはS3アカウントの接続

必要な資格情報を収集したら、次の手順に従って、接続先の新しいBlobまたはS3アカウントを作成でき [!DNL Platform]ます。

「 [Adobe Experience Platform](https://platform.adobe.com) 」にログインし、左のナビゲーションバーで「 **[!UICONTROL ソース]** 」を選択して「 *[!UICONTROL ソース]* 」ワークスペースにアクセスします。 「 *[!UICONTROL カタログ]* 」画面には様々なソースが表示され、このソースを使用してインバウンドアカウントを作成できます。各ソースには、関連付けられた既存のアカウントおよびデータフローの数が表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「 *[!UICONTROL Databases]***[!UICONTROL カテゴリ」で、「]** Azure Databasesストレージ **[!UICONTROL 」または「]** Amazon Amazon Amazon3 ****[!DNL Blob] 」を選択し、「+」アイコン(+)をクリックして、新しいDatabasesまたはAmazon s3コネクタを作成します。

![カタログ](../../../../images/tutorials/create/blob/catalog.png)

[ *[!UICONTROL Azure BLOBストレージに]* 接続]ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、接続に名前、オプションの説明、およびS3の資格情報を入力し [!DNL Blob] ます。 完了したら、[ **[!UICONTROL 接続]** ]を選択し、新しいアカウントが確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/blob/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するS3アカウント [!DNL Blob] またはS3アカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/blob/existing.png)

## 次の手順とその他のリソース

このチュートリアルに従って、 [!DNL Blob] またはS3アカウントへの接続を確立しました。 次のチュートリアルに進み、クラウドストレージのデータをPlatformに取り込むようにデータフローを [設定できます](../../dataflow/batch/cloud-storage.md)。