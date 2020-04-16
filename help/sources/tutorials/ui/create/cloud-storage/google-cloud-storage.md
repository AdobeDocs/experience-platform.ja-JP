---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのGoogle Cloudストレージソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: f09ff4d1b159a6989868c5cfc35b361cfb640a99

---


# UIでのGoogle Cloudストレージソースコネクタの作成

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、Platformユーザーインターフェイスを使用してGoogle Cloudストレージ（以下「GCS」と呼ばれる）のソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分な理解を得る必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md):XDMスキーマの基本的な構成要素について説明します。この中には、主な原則や構成のベストプラクティスが含まれています。スキーマ構成の基本要素です。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):カスタムスキーマを作成する方法についてスキーマエディターのUI。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。

GCSベースの接続が既にある場合は、このドキュメントの残りの部分をスキップし、データフローの設定に関するチュートリアルに進む [ことができます](../../dataflow/cloud-storage.md)。

### サポートされているファイル形式

Experience Platformは、外部プラットフォームから取り込む次のファイル形式をサポートしています。ストレージ

* 区切り文字区切り値(DSV):DSV形式のデータ・ファイルのサポートは、現在、コンマ区切り値に制限されています。 DSV形式のファイル内のフィールド・ヘッダーの値は、英数字とアンダースコアのみで構成する必要があります。 将来、一般的なDSVファイルのサポートが提供されます。
* JavaScript Object Notation (JSON):JSON形式のデータファイルはXDMに準拠している必要があります。
* Apache Parket:パーケット形式のデータファイルは、XDMに準拠している必要があります。

### 必要な資格情報の収集

プラットフォーム上のGCSデータにアクセスするには、有効なGCSアクセスキーIDとシークレットを **指定する** 必要があ **ります**。 これらの値の取得方法について詳しくは、Google Cloudのサーバー間 <a href="https://cloud.google.com/docs/authentication/production" target="_blank">認証ガイドを参照して</a> 、詳しく説明します。

## GCSアカウントの接続

必要な資格情報を収集したら、次の手順に従って新しい受信ベース接続を作成し、GCSアカウントをプラットフォームにリンクします。

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし</a> 、左のナビゲーションバーから「 **Sources** 」を選択して、ソースワークスペースにアク *セスします* 。 カタ *ログ画面には* 、様々なソースが表示されます。このソースを使用して受信ベース接続を作成でき、各ソースに関連付けられた既存のベース接続の数が表示されます。

「クラウ *ドストレージ* カテゴリ」で、「 **Google Cloudストレージ** 」を選択し、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソース表示のドキュメントに接続するか、ソースに接続するかのオプションが表示されます。 新しい受信ベース接続を作成するには、[ソースの接続]を **クリックしま**&#x200B;す。

![](../../../../images/tutorials/create/google-cloud-storage/sources-catalog.png)

「Google Cloud _に接続」ストレージ_ ・ダイアログが表示されます。 入力フォームで、ベース接続に名前、オプションの説明、およびGCS秘密鍵証明書を指定します。 終了したら、[接続 **** ]をクリックし、新しいベース接続が確立されるまでの時間を設定します。

![](../../../../images/tutorials/create/google-cloud-storage/gcs-credentials.png)

ベース接続が確立されたら、次のセクションに進み、データをプラットフォームに取り込むようにデータフローを設定できます。

## 次の手順

このチュートリアルに従って、GCSアカウントへの基本接続を確立しました。 次のチュートリアルに進み、データをプラットフォームに [取り込むようにデータフローを設定できます](../../dataflow/cloud-storage.md)。