---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでPhoenixソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 5ad763d2167c68f3293a2813248efaee22230a52
workflow-type: tm+mt
source-wordcount: '574'
ht-degree: 0%

---


# UIでPhoenixソースコネクタを作成する

> [!NOTE]
> フェニックスコネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、Platformのユーザインターフェイスを使用してPhoenixソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、次のAdobe Experience Platformのコンポーネントについて十分に理解している必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md): Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成における主な原則とベストプラクティスが含まれます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効なPhoenix接続をお持ちの場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/databases.md)

### 必要な資格情報の収集

Platform上のPhoenixアカウントにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | PhoenixサーバーのIPアドレスまたはホスト名。 |
| `port` | Phoenixサーバーがクライアント接続のリッスンに使用するTCPポートです。 Azure HDInsightsに接続する場合は、ポートを443に指定します。 |
| `httpPath` | Phoenixサーバーに対応する部分的なURLです。 Azure HDInsightsクラスターを使用する場合は、/hbasephoenix0を指定します。 |
| `username` | Phoenixサーバーへのアクセスに使用するユーザー名です。 |
| `password` | ユーザーに対応するパスワードです。 |
| `enableSsl` | サーバーへの接続がSSLを使用して暗号化されるかどうかを指定するトグル。 |

使い始める前に詳しくは、 [このフェニックスドキュメントを参照してください](https://python-phoenixdb.readthedocs.io/en/latest/api.html)。

## Phoenixアカウントの接続

必要な資格情報を収集したら、次の手順に従って新しいPhoenixアカウントを作成し、Platformに接続します。

「 <a href="https://platform.adobe.com" target="_blank">Adobe Experience Platform</a> 」にログインし、左のナビゲーションバーで「 **ソース** 」を選択して「 *ソース* 」ワークスペースにアクセスします。 カ *タログ* 画面には様々なソースが表示され、このソースから受信アカウントを作成できます。各ソースには既存のアカウントの数と関連するデータセットフローが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

[ *Databases* ] **カテゴリの[** Phoenix]を選択して、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 新しい受信接続を作成するには、「 **接続ソース**」を選択します。

![カタログ](../../../../images/tutorials/create/phoenix/catalog.png)

「Phoenix *に接続* 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **新規アカウント**」を選択します。 表示される入力フォームで、名前、オプションの説明、Phoenixの資格情報を付けて接続を指定します。 完了したら、[ **接続** ]を選択し、新しいアカウントが確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/phoenix/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するPhoenixアカウントを選択し、「 **次へ** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/phoenix/existing.png)

## 次の手順

このチュートリアルに従って、Phoenixアカウントとの接続を確立しました。 次のチュートリアルに進み、データをPlatformに取り込むようにデータフローを [設定できます](../../dataflow/databases.md)。