---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのMySQLソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: f09ff4d1b159a6989868c5cfc35b361cfb640a99

---


# UIでのMySQLソースコネクタの作成

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してMySQLソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分な理解を得る必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md):XDMスキーマの基本的な構成要素について説明します。この中には、主な原則や構成のベストプラクティスが含まれています。スキーマ構成の基本要素です。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):カスタムスキーマを作成する方法についてスキーマエディターのUI。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。

MySQLベースの接続が既にある場合は、このドキュメントの残りの部分をスキップし、データフローの設定に関するチュートリアルに進む [ことができます](../../dataflow/databases.md)。

### 必要な資格情報の収集

プラットフォーム上のMySQLアカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | アカウントに関連付けられているMySQL接続文字列。 |

接続文字列の詳細と、その取得方法については、 [MySQLドキュメント](https://dev.mysql.com/doc/connector-net/en/connector-net-connections-string.html)。

## MySQLアカウントの接続

必要な資格情報を収集したら、次の手順に従って、新しい受信ベース接続を作成し、MySQLアカウントをプラットフォームにリンクできます。

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし</a> 、左のナビゲーションバーから「 **Sources** 」を選択して、ソースワークスペースにアク *セスします* 。 カタ *ログ画面には* 、様々なソースが表示されます。このソースを使用して受信ベース接続を作成でき、各ソースに関連付けられた既存のベース接続の数が表示されます。

[データベ *ース* ]カテゴリの下で[ **MySQL** ]を選択し、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたはソースのドキュメントに接続するための表示が表示されます。 新しい受信ベース接続を作成するには、[ソースの接続] **を選択します**。

![](../../../../images/tutorials/create/my-sql/sources-catalog.png)

[MySQL *に接続* ]ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「新規アカウント」 **を選択しま**&#x200B;す。 表示される入力フォームで、ベース接続に名前、オプションの説明、MySQL資格情報を指定します。 終了したら、[接続 **** ]を選択し、新しいベース接続が確立されるまでの時間を設定します。

![](../../../../images/tutorials/create/my-sql/new-credentials.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するMySQLアカウントを選択し、「次へ」を選 **択し** 、先に進みます。

![](../../../../images/tutorials/create/my-sql/existing-credentials.png)

## 次の手順

このチュートリアルに従って、MySQLアカウントへの基本接続を確立しました。 次のチュートリアルに進み、データをプラットフォームに [取り込むようにデータフローを設定できます](../../dataflow/databases.md)。