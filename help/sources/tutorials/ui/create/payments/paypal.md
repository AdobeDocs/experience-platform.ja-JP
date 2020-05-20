---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのPayPalソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: 2162c66b1664ecaaf0b609fe3f7ccf58c4a5d31d
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 1%

---


# UIでのPayPalソースコネクタの作成

> [!NOTE]
> PayPalコネクタはベータ版です。 機能とドキュメントは、変更されることがあります。

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してPayPalソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成の主な原則とベストプラクティスが含まれます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既にPayPalベースの接続をお持ちの場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/payments.md)

### 必要な資格情報の収集

PayPalアカウントプラットフォームにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | PayPalインスタンスのURL。 |
| `clientID` | PayPalアプリケーションに関連付けられているクライアントID。 |
| `clientSecret` | PayPalアプリケーションに関連付けられているクライアントシークレット。 |

使い始める前に、このPayPalのドキュメントを参照して [ください](https://developer.paypal.com/docs/api/overview/#get-credentials)

## PayPalアカウントの接続

必要な資格情報を収集したら、次の手順に従って、PayPalアカウントをプラットフォームにリンクする新しい受信ベース接続を作成できます。

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし、左のナビゲーションバーで「</a> Sources **** 」を選択して *Sources* ワークスペースにアクセスします。 [ *カタログ* ]画面には、様々なソースが表示され、このソースを使用して受信ベース接続を作成できます。各ソースには、それらに関連付けられた既存のベース接続の数が表示されます。

「 *CRM* カテゴリ」の下で、「 **PayPal** 」を選択して、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 新しい受信ベース接続を作成するには、「 **接続ソース**」を選択します。

![カタログ](../../../../images/tutorials/create/paypal/catalog.png)

PayPalに *接続* ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **新規アカウント**」を選択します。 表示される入力フォームで、基本接続に名前、オプションの説明、PayPal資格情報を入力します。 終了したら、[ **接続** ]を選択し、新しいベース接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/paypal/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するPayPalアカウントを選択し、「 **次へ** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/paypal/existing.png)

## 次の手順

このチュートリアルに従うと、PayPalアカウントとの基本的な接続が確立されます。 次のチュートリアルに進み、CRMデータをプラットフォームに取り込むためのデータフローを [設定できるようになりました](../../dataflow/payments.md)。