---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでPhoenixソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: f09ff4d1b159a6989868c5cfc35b361cfb640a99

---


# UIでPhoenixソースコネクタを作成する

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してPhoenixソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分な理解を得る必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md):XDMスキーマの基本的な構成要素について説明します。この中には、主な原則や構成のベストプラクティスが含まれています。スキーマ構成の基本要素です。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):カスタムスキーマを作成する方法についてスキーマエディターのUI。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。

既に有効なPhoenix接続をお持ちの場合は、このドキュメントの残りの部分をスキップし、データフローの設定に関するチュートリアルに進む [ことができます。](../../dataflow/databases.md)

### 必要な資格情報の収集

プラットフォーム上のPhoenixアカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | PhoenixサーバーのIPアドレスまたはホスト名。 |
| `port` | Phoenixサーバーがクライアント接続のリッスンに使用するTCPポートです。 Azure HDInsightsに接続する場合は、ポートを443に指定します。 |
| `httpPath` | Phoenixサーバーに対応する部分的なURL。 Azure HDInsightsクラスターを使用する場合は、/hbasephoenix0を指定します。 |
| `username` | Phoenixサーバーへのアクセスに使用するユーザー名。 |
| `password` | ユーザーに対応するパスワードです。 |
| `enableSsl` | サーバーへの接続をSSLを使用して暗号化するかどうかを指定する切り替え。 |

使い始めの詳細は、このフェニックス [ドキュメント](https://python-phoenixdb.readthedocs.io/en/latest/api.html)。

## Phoenixアカウントの接続

必要な資格情報を収集したら、次の手順に従って新しいPhoenixアカウントを作成し、プラットフォームに接続します。

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし</a> 、左のナビゲーションバーから「 **Sources** 」を選択して、ソースワークスペースにアク *セスします* 。 カタロ *グ画面には* 、様々なソースが表示され、このソースに対してインバウンドアカウントを作成できます。各ソースには、既存のアカウントの数と、それらに関連付けられたデータセットフローが表示されます。

画面の左側のカテゴリから適切なカタログを選択できます。 または、検索オプションを使用して、作業対象の特定のソースを見つけることもできます。

[データベ *ース* ]カテゴリの[ **Phoenix** ]を選択し、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたはソースのドキュメントに接続するための表示が表示されます。 新しい受信接続を作成するには、「 **Connect source**」を選択します。

![カタログ](../../../../images/tutorials/create/phoenix/catalog.png)

「Connect *to Phoenix* 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「新規アカウント」 **を選択しま**&#x200B;す。 表示される入力フォームで、接続に名前、オプションの説明、Phoenixの資格情報を指定します。 完了したら、[ **Connect** ]を選択し、新しいアカウントが確立されるまでの時間を設定します。

![接続](../../../../images/tutorials/create/phoenix/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するPhoenixアカウントを選択し、「次へ」を選 **択し** 、先に進みます。

![既存](../../../../images/tutorials/create/phoenix/existing.png)

## 次の手順

このチュートリアルに従って、Phoenixアカウントとの接続を確立しました。 次のチュートリアルに進み、データをプラットフォームに [取り込むようにデータフローを設定できます](../../dataflow/databases.md)。