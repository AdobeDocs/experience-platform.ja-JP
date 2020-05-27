---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのGoogle AdWordsソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: e7211c9ebfd8fecff3780198d71e18436f3ffab3
workflow-type: tm+mt
source-wordcount: '519'
ht-degree: 0%

---


# UIでのGoogle AdWordsソースコネクタの作成

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してGoogle AdWordsソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成における主な原則とベストプラクティスが含まれます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既にGoogle AdWords接続をお持ちの場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/payments.md)

### 必要な資格情報の収集

Google AdWordsアカウントプラットフォームにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `clientCustomerId` | AdWordsアカウントのクライアント顧客ID。 |
| `developerToken` | マネージャーアカウントに関連付けられている開発者トークン。 |
| `refreshToken` | AdWordsへのアクセスを承認するためにGoogleから取得した更新トークン。 |
| `clientId` | 更新トークンの取得に使用されるGoogleアプリケーションのクライアントID。 |
| `clientSecret` | 更新トークンの取得に使用されるGoogleアプリケーションのクライアントシークレット。 |

使い始める前に、この [Google AdWordsドキュメントを参照してください](https://developers.google.com/adwords/api/docs/guides/authentication)。

## Google AdWordsアカウントの接続

必要な資格情報を収集したら、次の手順に従って新しい受信ベース接続を作成し、Google AdWordsアカウントをプラットフォームにリンクします。

[Adobe Experience Platformにログインし、左のナビゲーションバーで「](https://platform.adobe.com) Sources **** 」を選択して *Sources* ワークスペースにアクセスします。 [ *カタログ* ]画面には、様々なソースが表示され、このソースを使用して受信ベース接続を作成できます。各ソースには、それらに関連付けられた既存のベース接続の数が表示されます。

「 *広告* 」カテゴリの下で「 **Google AdWords** 」を選択し、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 新しい受信ベース接続を作成するには、「 **接続ソース**」を選択します。

![カタログ](../../../../images/tutorials/create/ads/catalog.png)

「Google AdWords *に接続* 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **新規アカウント**」を選択します。 表示される入力フォームで、基本接続に名前、オプションの説明およびGoogle AdWordsの資格情報を指定します。 終了したら、[ **接続** ]を選択し、新しいベース接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/ads/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するGoogle AdWordsアカウントを選択し、「 **次へ** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/ads/existing.png)

## 次の手順

このチュートリアルに従って、Google AdWordsアカウントへの基本的な接続を確立しました。 次のチュートリアルに進み、広告データをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/advertising.md)。