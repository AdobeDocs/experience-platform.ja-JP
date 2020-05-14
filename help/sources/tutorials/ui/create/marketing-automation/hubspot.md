---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのHubSpotソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: 2162c66b1664ecaaf0b609fe3f7ccf58c4a5d31d
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 0%

---


# UIでのHubSpotソースコネクタの作成

> [!NOTE]
> HubSpotコネクタはベータ版です。 機能とドキュメントは、変更されることがあります。

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してHubSpotソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成の主な原則とベストプラクティスが含まれます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既にHubSpotベースの接続をお持ちの場合は、このドキュメントの残りの部分をスキップし、マーケティング自動化データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/marketing-automation.md)。

### 必要な資格情報の収集

プラットフォームのHubSpotアカウントにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `clientId` | HubSpotアプリケーションに関連付けられているクライアントID。 |
| `clientSecret` | HubSpotアプリケーションに関連付けられているクライアントシークレット。 |
| `accessToken` | OAuth統合を最初に認証する際に取得されるアクセストークン。 |
| `refreshToken` | OAuth統合を最初に認証する際に取得される更新トークン。 |

使い始める前に、このHubSpotドキュメントを参照して [ください](https://developers.hubspot.com/docs/methods/oauth2/oauth2-overview)。

## HubSpotアカウントの接続

必要な資格情報を収集したら、次の手順に従って新しい受信ベース接続を作成し、HubSpotアカウントをプラットフォームにリンクします。

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし、左のナビゲーションバーで「</a> Sources **** 」を選択して *Sources* ワークスペースにアクセスします。 [ *カタログ* ]画面には、様々なソースが表示され、このソースを使用して受信ベース接続を作成できます。各ソースには、それらに関連付けられた既存のベース接続の数が表示されます。

「 *Marketing automation* 」カテゴリの下で「 **HubSpot** 」を選択し、情報バーを画面の右側に表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 新しい受信ベース接続を作成するには、「 **接続ソース**」を選択します。

![カタログ](../../../../images/tutorials/create/hubspot/catalog.png)

HubSpot *に接続* ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **新規アカウント**」を選択します。 表示される入力フォームで、基本接続に名前、オプションの説明およびHubSpot資格情報を入力します。 終了したら、[ **接続** ]を選択し、新しいベース接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/hubspot/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するHubSpotアカウントを選択し、「 **次へ** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/hubspot/existing.png)

## 次の手順

このチュートリアルに従って、HubSpotアカウントへの基本接続を確立しました。 次のチュートリアルに進み、マーケティング自動化システムのデータをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/marketing-automation.md)。