---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでAzure BlobまたはAmazon S3ソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: f09ff4d1b159a6989868c5cfc35b361cfb640a99

---


# UIでAzure BlobまたはAmazon S3ソースコネクタを作成する

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してAzure Blob（以下「Blob」と呼びます）またはAmazon S3（以下「S3」と呼びます）のソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分な理解を得る必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md):XDMスキーマの基本的な構成要素について説明します。この中には、主な原則や構成のベストプラクティスが含まれています。スキーマ構成の基本要素です。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):カスタムスキーマを作成する方法についてスキーマエディターのUI。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。

既にBLOBまたはS3ベース接続をお持ちの場合は、このドキュメントの残りの部分をスキップし、データフローの設定に関するチュートリアルに進む [ことができます](../../dataflow/cloud-storage.md)。

### サポートされているファイル形式

Experience Platformは、外部プラットフォームから取り込む次のファイル形式をサポートしています。ストレージ

* 区切り文字区切り値(DSV):DSV形式のデータ・ファイルのサポートは、現在、コンマ区切り値に制限されています。 DSV形式のファイル内のフィールド・ヘッダーの値は、英数字とアンダースコアのみで構成する必要があります。 将来、一般的なDSVファイルのサポートが提供されます。
* JavaScript Object Notation (JSON):JSON形式のデータファイルはXDMに準拠している必要があります。
* Apache Parket:パーケット形式のデータファイルは、XDMに準拠している必要があります。

### 必要な資格情報の収集

プラットフォーム上のBLOBストレージにアクセスするには、有効な **Azureストレージ接続文字列を指定する必要があります**。 このMicrosoft Azureドキュメントを使用して接続文字列を取得する方法など、接続文字列の詳細 <a href="https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string" target="_blank">を確認できます</a>。

同様に、プラットフォーム上のS3バケットにアクセスするには、 **S3アクセスキーと** S3シークレットキーを提供する **必要があります**。 For more information, refer to <a href="https://aws.amazon.com/blogs/security/wheres-my-secret-access-key/" target="_blank">this AWS document</a>.

## BlobまたはS3アカウントの接続

クラウドストレージの資格情報の準備が整ったら、次の手順に従って新しい受信ベース接続を作成し、BlobまたはS3アカウントをプラットフォームにリンクできます。

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし</a> 、左のナビゲーションバーから「 **Sources** 」を選択して、ソースワークスペースにアクセスします。 カタ *ログ画面には* 、様々なソースが表示されます。このソースを使用して受信ベース接続を作成でき、各ソースに関連付けられた既存のベース接続の数が表示されます。

*Cloudストレージ* カテゴリの下で、「 **Azure Blobストレージ** 」または「 **Amazon S3** 」を選択し、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ドキュメントを表示したり、ソースに接続するためのオプションが表示されます。 新しい受信ベース接続を作成するには、[ソースの接続]を **クリックしま**&#x200B;す。

![](../../../../images/tutorials/create/s3/s3_sources_catalog.png)

入力フォームで、ベース接続に名前、オプションの説明、およびBLOBまたはS3の秘密鍵証明書を指定します。 最後に、[接続 **]をクリックし** 、新しいベース接続が確立されるまでの時間を設定します。

![](../../../../images/tutorials/create/s3/s3_credentials.png)

## 次の手順

このチュートリアルに従うと、Azure BlobまたはAmazon S3アカウントへのベース接続が確立されます。 次のチュートリアルに進み、データをプラットフォームに [取り込むようにデータフローを設定できます](../../dataflow/cloud-storage.md)。