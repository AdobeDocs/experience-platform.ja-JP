---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでMicrosoft DynamicsまたはSalesforceソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: f09ff4d1b159a6989868c5cfc35b361cfb640a99

---


# UIでMicrosoft DynamicsまたはSalesforceソースコネクタを作成する

Adobe Experience Platformのソースコネクタは、外部ソースのCRMデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してMicrosoft Dynamics（以下「Dynamics」と呼ばれる）またはSalesforceソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分な理解を得る必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md):XDMスキーマの基本的な構成要素について説明します。この中には、主な原則や構成のベストプラクティスが含まれています。スキーマ構成の基本要素です。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):カスタムスキーマを作成する方法についてスキーマエディターのUI。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。

既にMicrosoft DynamicsまたはSalesforceのベース接続をお持ちの場合は、このドキュメントの残りの部分をスキップし、データフローの設定に関するチュートリアルに進 [むことができます](../../dataflow/crm.md)。

### 必要な資格情報の収集

プラットフォーム上のDynamicsアカウントにアクセスするには、サービスURI **、ユーザー名****、パスワードを指定**&#x200B;します ****。

同様に、プラットフォーム上のSalesforceアカウントにアクセスするには、 ****&#x200B;環境URL **、ユーザ**&#x200B;名 **、パスワード**、セキュリティ ****&#x200B;トークンを指定します。

## DynamicsまたはSalesforceアカウントの接続

CRMシステムの資格情報の準備が整ったら、次の手順に従って新しい受信ベース接続を作成し、DynamicsまたはSalesforceアカウントをプラットフォームにリンクできます。

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし</a> 、左のナビゲーションバーから「 **Sources** 」を選択して、ソースワークスペースにアクセスします。 カタ *ログ画面には* 、様々なソースが表示されます。このソースを使用して受信ベース接続を作成でき、各ソースに関連付けられた既存のベース接続の数が表示されます。

*CRM* カテゴリの下で、 **Microsoft Dynamics** 、 **Salesforce** のいずれかを選択して、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ドキュメントを表示したり、ソースに接続するためのオプションが表示されます。 新しい受信ベース接続を作成するには、[ソースの接続]を **クリックしま**&#x200B;す。

![](../../../../images/tutorials/create/salesforce/sf_sources_catalog.png)

入力フォームで、ベース接続に名前、オプションの説明、およびDynamicsまたはSalesforceの資格情報を指定します。 最後に、[接続 **]をクリックし** 、新しいベース接続が確立されるまでの時間を設定します。

![](../../../../images/tutorials/create/salesforce/sf_credentials.png)

CRMシステムとのベース接続が確立されたら、次のセクションに進み、CRMデータをプラットフォームに取り込むようにデータフローを設定できます。

## 次の手順

このチュートリアルに従うと、DynamicsまたはSalesforceアカウントへの基本接続が確立されます。 次のチュートリアルに進み、データをプラットフォームに [取り込むようにデータフローを設定できます](../../dataflow/crm.md)。