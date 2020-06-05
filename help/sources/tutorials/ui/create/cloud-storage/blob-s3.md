---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIにAzure BlobまたはAmazon S3ソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 9fd00ec198f61843bb9a395103215e5441b23745
workflow-type: tm+mt
source-wordcount: '644'
ht-degree: 1%

---


# UIにAzure BlobまたはAmazon S3ソースコネクタを作成する

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してAzure Blob （以下「BLOB」と呼びます）またはAmazon S3 （以下「S3」と呼びます）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

- [Experience Data Model(XDM)System](../../../../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成の主な原則とベストプラクティスが含まれます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既にBlobまたはS3ベースの接続がある場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/batch/cloud-storage.md)。

### サポートされているファイル形式

Experience Platformは、次のファイル形式をサポートしており、外部ストレージから取り込むことができます。

- 区切り文字区切り値(DSV): DSV形式のデータ・ファイルのサポートは、現在、コンマ区切り値に制限されています。 DSV形式のファイル内のフィールド・ヘッダーの値は、英数字とアンダースコアのみで構成する必要があります。 一般的なDSVファイルは、今後サポートされる予定です。
- JavaScript Object Notation (JSON): JSON形式のデータファイルは、XDMに準拠している必要があります。
- Apacheパーケット： パーケット形式のデータファイルは、XDMに準拠している必要があります。

### 必要な資格情報の収集

プラットフォームのBLOBストレージにアクセスするには、次の秘密鍵証明書の有効な値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | BLOBストレージのデータにアクセスするために必要な接続文字列です。 BLOB接続文字列パターンは次のとおりです。 `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`. |

開始方法の詳細については、 [このAzure Blobドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string)。

同様に、プラットフォーム上のS3バケットにアクセスするには、次の資格情報の有効値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `s3AccessKey` | S3ストレージのアクセスキーID。 |
| `s3SecretKey` | S3ストレージの秘密鍵ID。 |

開始方法の詳細については、 [このAWSドキュメントを参照してください](https://aws.amazon.com/blogs/security/wheres-my-secret-access-key/)。

## BlobまたはS3アカウントの接続

必要な資格情報を収集したら、次の手順に従って新しいBlobまたはS3アカウントを作成し、プラットフォームに接続します。

[Adobe Experience Platformにログインし、左のナビゲーションバーで「](https://platform.adobe.com) Sources **** 」を選択して *[!UICONTROL Sources]* ワークスペースにアクセスします。 「 *[!UICONTROL カタログ]* 」画面には様々なソースが表示され、このソースを使用してインバウンドアカウントを作成できます。各ソースには、関連付けられた既存のアカウントおよびデータフローの数が表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「 *[!UICONTROL Databases]* 」 **[!UICONTROL カテゴリで、「]** Azure Databasesストレージ **[!UICONTROL 」または「]** Amazon S3 **** 」を選択し、「+」アイコン(+)をクリックして、新しいBlobまたはS3コネクタを作成します。

![カタログ](../../../../images/tutorials/create/blob/catalog.png)

[ *[!UICONTROL Azure BLOBストレージに]* 接続]ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、接続に名前、オプションの説明、およびBlobまたはS3秘密鍵証明書を入力します。 完了したら、「 **[!UICONTROL 接続]** 」を選択し、新しいアカウントが確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/blob/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するBLOBまたはS3アカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/blob/existing.png)

## 次の手順

このチュートリアルに従って、BlobまたはS3アカウントへの接続を確立しました。 次のチュートリアルに進み、クラウドストレージのデータをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/batch/cloud-storage.md)。