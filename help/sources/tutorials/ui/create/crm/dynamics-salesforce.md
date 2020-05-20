---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでMicrosoft DynamicsまたはSalesforceソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: f09ff4d1b159a6989868c5cfc35b361cfb640a99
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 0%

---


# UIでMicrosoft DynamicsまたはSalesforceソースコネクタを作成する

Adobe Experience Platformのソースコネクターは、外部ソースのCRMデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してMicrosoft Dynamics（以下「Dynamics」と呼びます）またはSalesforceソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成の主な原則とベストプラクティスが含まれます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既にMicrosoft DynamicsまたはSalesforceベースの接続をお持ちの場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/crm.md)。

### 必要な資格情報の収集

プラットフォームのDynamicsアカウントにアクセスするには、 **サービスURI**、 **ユーザー名**、 **パスワードを指定する必要があります**。

同様に、プラットフォーム上のSalesforceアカウントにアクセスするには、 **環境URL、**&#x200B;ユーザ名 **、パスワード****、******&#x200B;セキュリティトークンを入力する必要があります。

## DynamicsまたはSalesforceアカウントの接続

CRMシステムの資格情報の準備ができたら、次の手順に従って、DynamicsまたはSalesforceアカウントをプラットフォームにリンクする新しい受信ベース接続を作成できます。

Adobe Experience Platformにログインし、左のナビゲーションバーで「 <a href="https://platform.adobe.com" target="_blank">Sources</a>**** 」を選択してソースワークスペースにアクセスします。 [ *カタログ* ]画面には、様々なソースが表示され、このソースを使用して受信ベース接続を作成できます。各ソースには、それらに関連付けられた既存のベース接続の数が表示されます。

「 *CRM* カテゴリ」で、「 **Microsoft Dynamics** 」または「 **Salesforce** 」を選択して、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ドキュメントの表示やソースへの接続に関するオプションが表示されます。 新しい受信ベース接続を作成するには、[ **接続元**]をクリックします。

![](../../../../images/tutorials/create/salesforce/sf_sources_catalog.png)

入力フォームで、基本接続に名前、オプションの説明、DynamicsまたはSalesforceの資格情報を入力します。 最後に、[ **接続** ]をクリックし、新しいベース接続が確立されるまでの時間をお待ちください。

![](../../../../images/tutorials/create/salesforce/sf_credentials.png)

CRMシステムとの基本的な接続が確立されたら、次のセクションに進み、CRMデータをプラットフォームに取り込むようにデータフローを設定できます。

## 次の手順

このチュートリアルに従うことで、DynamicsまたはSalesforceアカウントへの基本接続を確立できました。 次のチュートリアルに進み、データをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/crm.md)。