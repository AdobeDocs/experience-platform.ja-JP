---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでAzure HDInsightsソースコネクタ上にApache Sparkを作成します
topic: overview
translation-type: tm+mt
source-git-commit: ae059f93f09dbbae4f1ef46f68901071afba9729

---


# UIでAzure HDInsightsソースコネクタ上にApache Sparkを作成します

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、プラットフォームユーザーインターフェイスを使用してAzure HDInsightsソースコネクタ上にApache Sparkを作成する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分な理解を得る必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md):XDMスキーマの基本的な構成要素について説明します。この中には、主な原則や構成のベストプラクティスが含まれています。スキーマ構成の基本要素です。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):カスタムスキーマを作成する方法についてスキーマエディターのUI。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。

既に有効なSpark接続がある場合は、このドキュメントの残りの部分をスキップし、データフローの設定に関するチュートリアルに進む [ことができます](../../dataflow/databases.md)

### 必要な資格情報の収集

プラットフォーム上のSparkアカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | SparkサーバーのIPアドレスまたはホスト名。 |
| `username` | Sparkサーバーへのアクセスに使用するユーザー名です。 |
| `password` | ユーザーに対応するパスワードです。 |

使い始める方法の詳細については、このSparkの [ドキュメント](https://docs.microsoft.com/en-us/azure/hdinsight/spark/apache-spark-overview)。

## Sparkアカウントの接続

必要な資格情報を収集したら、次の手順に従って新しいSparkアカウントを作成し、Platformに接続します。

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし</a> 、左のナビゲーションバーから「 **Sources** 」を選択して、ソースワークスペースにアク *セスします* 。 カタロ *グ画面には* 、様々なソースが表示され、このソースに対してインバウンドアカウントを作成できます。各ソースには、既存のアカウントの数と、それらに関連付けられたデータセットフローが表示されます。

画面の左側のカテゴリから適切なカタログを選択できます。 または、検索オプションを使用して、作業対象の特定のソースを見つけることもできます。

「 *Databases* 」カテゴリの下で、 **Spark** を選択して、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたはソースのドキュメントに接続するための表示が表示されます。 新しい受信接続を作成するには、「 **Connect source**」を選択します。

![カタログ](../../../../images/tutorials/create/spark/catalog.png)

「 *Connect to Spark* 」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「新規アカウント」 **を選択しま**&#x200B;す。 表示される入力フォームで、接続に名前、オプションの説明、Sparkの秘密鍵証明書を指定します。 完了したら、[ **Connect** ]を選択し、新しいアカウントが確立されるまでの時間を設定します。

![新規](../../../../images/tutorials/create/spark/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するSparkアカウントを選択し、「次へ」を選択 **して** 、先に進みます。

![既存](../../../../images/tutorials/create/spark/existing.png)

## 次の手順

このチュートリアルに従って、Sparkアカウントへの接続を確立しました。 次のチュートリアルに進み、データをプラットフォームに [取り込むようにデータフローを設定できます](../../dataflow/databases.md)。