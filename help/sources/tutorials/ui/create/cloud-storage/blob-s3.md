---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIにAzure BlobまたはAmazon S3ソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 799445eca080175e2bffc49c6714f0c812b9bbea
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 1%

---


# UIにAzure BlobまたはAmazon S3ソースコネクタを作成する

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してAzure Blob （以下「BLOB」と呼びます）またはAmazon S3 （以下「S3」と呼びます）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

- [Experience Data Model(XDM)System](../../../../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成における主な原則とベストプラクティスが含まれます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既にBlobまたはS3ベースの接続がある場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/batch/cloud-storage.md)。

### サポートされているファイル形式

Experience Platformは、次のファイル形式をサポートしており、外部ストレージから取り込むことができます。

- 区切り文字区切り値(DSV): DSV形式のデータ・ファイルのサポートは、現在、コンマ区切り値に制限されています。 DSV形式のファイル内のフィールド・ヘッダーの値は、英数字とアンダースコアのみで構成する必要があります。 一般的なDSVファイルは、今後サポートされる予定です。
- JavaScript Object Notation (JSON): JSON形式のデータファイルは、XDMに準拠している必要があります。
- Apacheパーケット： パーケット形式のデータファイルは、XDMに準拠している必要があります。

### 必要な資格情報の収集

プラットフォームのBLOBストレージにアクセスするには、有効な **Azureストレージ接続文字列を指定する必要があります**。 このMicrosoft Azureドキュメントを使用して接続文字列を取得する方法など、接続文字列の詳細 <a href="https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string" target="_blank">を確認できます</a>。

同様に、プラットフォーム上のS3バケットにアクセスするには、 **S3アクセスキー** と **S3シークレットキーを指定する必要があります**。 For more information, refer to <a href="https://aws.amazon.com/blogs/security/wheres-my-secret-access-key/" target="_blank">this AWS document</a>.

## BlobまたはS3アカウントの接続

クラウドストレージの資格情報の準備が整ったら、次の手順に従って新しい受信ベース接続を作成し、BlobまたはS3アカウントをプラットフォームにリンクできます。

Adobe Experience Platformにログインし、左のナビゲーションバーで「 <a href="https://platform.adobe.com" target="_blank">Sources</a>**** 」を選択してソースワークスペースにアクセスします。 [ *カタログ* ]画面には、様々なソースが表示され、このソースを使用して受信ベース接続を作成できます。各ソースには、それらに関連付けられた既存のベース接続の数が表示されます。

「 *Cloudストレージ* 」カテゴリで、「 **Azure Blobストレージ** 」または「 **Amazon S3** 」を選択して、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ドキュメントの表示やソースへの接続に関するオプションが表示されます。 新しい受信ベース接続を作成するには、[ **接続元**]をクリックします。

![](../../../../images/tutorials/create/s3/s3_sources_catalog.png)

入力フォームで、基本接続に名前、オプションの説明、およびBlobまたはS3秘密鍵証明書を入力します。 最後に、[ **接続** ]をクリックし、新しいベース接続が確立されるまでの時間をお待ちください。

![](../../../../images/tutorials/create/s3/s3_credentials.png)

## 次の手順

このチュートリアルに従って、Azure BlobまたはAmazon S3アカウントへの基本接続を確立しました。 次のチュートリアルに進み、データをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/batch/cloud-storage.md)。