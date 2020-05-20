---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのGoogle広告ソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: 3b5821d641d35e1190ea9fecfd4def5beced6ecc
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 0%

---


# UIでのGoogle広告ソースコネクタの作成

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してGoogle広告のソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成の主な原則とベストプラクティスが含まれます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既にGoogle Adsベースの接続をお持ちの場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/payments.md)

### 必要な資格情報の収集

Google広告のアカウントプラットフォームにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `clientCustomerId` | 広告アカウントのクライアント顧客ID。 |
| `developerToken` | マネージャーアカウントに関連付けられている開発者トークン。 |
| `refreshToken` | 広告へのアクセスを承認するためにGoogleから取得した更新トークン。 |
| `clientId` | 更新トークンの取得に使用されるGoogleアプリケーションのクライアントID。 |
| `clientSecret` | 更新トークンの取得に使用されるGoogleアプリケーションのクライアントシークレット。 |

使い始める前に、この [Google Adsドキュメントを参照してください](https://developers.google.com/adwords/api/docs/guides/authentication)。

## Google広告アカウントの接続

必要な資格情報を収集したら、次の手順に従って新しい受信ベース接続を作成し、Google広告アカウントをプラットフォームにリンクします。

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし、左のナビゲーションバーで「</a> Sources **** 」を選択して *Sources* ワークスペースにアクセスします。 [ *カタログ* ]画面には、様々なソースが表示され、このソースを使用して受信ベース接続を作成できます。各ソースには、それらに関連付けられた既存のベース接続の数が表示されます。

「 *広告* 」カテゴリの下で「 **Google広告** 」を選択して、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 新しい受信ベース接続を作成するには、「 **接続ソース**」を選択します。

![カタログ](../../../../images/tutorials/create/ads/catalog.png)

「Google広告に *接続* 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **新規アカウント**」を選択します。 表示される入力フォームで、基本接続に名前、オプションの説明、Google広告資格情報を入力します。 終了したら、[ **接続** ]を選択し、新しいベース接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/ads/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するGoogle広告のアカウントを選択し、「 **次へ** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/ads/existing.png)

## 次の手順

このチュートリアルに従って、Google Adsアカウントへの基本的な接続を確立しました。 次のチュートリアルに進み、広告データをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/advertising.md)。