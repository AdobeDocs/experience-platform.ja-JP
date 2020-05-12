---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのGoogle Cloudストレージソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: 799445eca080175e2bffc49c6714f0c812b9bbea
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 1%

---


# UIでのGoogle Cloudストレージソースコネクタの作成

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してGoogle Cloudストレージ（以下「GCS」と呼ばれる）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成の主な原則とベストプラクティスが含まれます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

GCSベースの接続が既にある場合は、このドキュメントの残りの部分をスキップし、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/batch/cloud-storage.md)。

### サポートされているファイル形式

Experience Platformは、次のファイル形式をサポートしており、外部ストレージから取り込むことができます。

* 区切り文字区切り値(DSV): DSV形式のデータ・ファイルのサポートは、現在、コンマ区切り値に制限されています。 DSV形式のファイル内のフィールド・ヘッダーの値は、英数字とアンダースコアのみで構成する必要があります。 一般的なDSVファイルは、今後サポートされる予定です。
* JavaScript Object Notation (JSON): JSON形式のデータファイルは、XDMに準拠している必要があります。
* Apacheパーケット： パーケット形式のデータファイルは、XDMに準拠している必要があります。

### 必要な資格情報の収集

プラットフォーム上のGCSデータにアクセスするには、有効なGCS **アクセスキーID** と **シークレットを指定する必要があります**。 これらの値の取得方法について詳しくは、Google Cloudの <a href="https://cloud.google.com/docs/authentication/production" target="_blank">サーバー間認証ガイド</a> （英語）を参照してください。

## GCSアカウントの接続

必要な資格情報を収集したら、次の手順に従って新しい受信ベース接続を作成し、GCSアカウントをプラットフォームにリンクします。

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし、左のナビゲーションバーで「</a> Sources **** 」を選択して *Sources* ワークスペースにアクセスします。 [ *カタログ* ]画面には、様々なソースが表示され、このソースを使用して受信ベース接続を作成できます。各ソースには、それらに関連付けられた既存のベース接続の数が表示されます。

「 *クラウドストレージ* 」カテゴリの下で、「 **Googleクラウドストレージ** 」を選択し、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソース表示のドキュメントに接続するか、ソースに接続するかを選択できるオプションが表示されます。 新しい受信ベース接続を作成するには、[ **接続元**]をクリックします。

![](../../../../images/tutorials/create/google-cloud-storage/sources-catalog.png)

Google Cloudストレージ _に接続_ ダイアログが表示されます。 入力フォームで、基本接続に名前、オプションの説明、およびGCS秘密鍵証明書を入力します。 終了したら、[ **接続** ]をクリックし、新しいベース接続が確立されるまでの時間をお待ちください。

![](../../../../images/tutorials/create/google-cloud-storage/gcs-credentials.png)

ベース接続が確立されたら、次のセクションに進み、データをプラットフォームに取り込むようにデータフローを設定できます。

## 次の手順

このチュートリアルに従って、GCSアカウントへの基本的な接続を確立しました。 次のチュートリアルに進み、データをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/batch/cloud-storage.md)。