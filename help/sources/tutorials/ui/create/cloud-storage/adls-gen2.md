---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでAzure Data LakeストレージGen2ソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: f09ff4d1b159a6989868c5cfc35b361cfb640a99

---


# UIでAzure Data LakeストレージGen2ソースコネクタを作成する

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、Azure Data LakeストレージGen2 （以下「ADLS Gen2」と呼ばれる）ソースコネクタをプラットフォームユーザーインターフェイスを使用して認証する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分な理解を得る必要があります。

- [Experience Data Model(XDM)System](../../../../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md):XDMスキーマの基本的な構成要素について説明します。この中には、主な原則や構成のベストプラクティスが含まれています。スキーマ構成の基本要素です。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):カスタムスキーマを作成する方法についてスキーマエディターのUI。
- [リアルタイム顧客プロファイル](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。

ADLS Gen2ベース接続を既にお持ちの場合は、このドキュメントの残りの部分をスキップし、データフローの設定に関するチュートリアルに進む [ことができます](../../dataflow/cloud-storage.md)。

### 必要な資格情報の収集

ADLS Gen2ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `url` | ADLS Gen2のエンドポイントです。 |
| `servicePrincipalId` | アプリケーションのクライアントID。 |
| `servicePrincipalKey` | アプリのキー。 |
| `tenant` | アプリケーションを含むテナント情報。 |

これらの値の詳細については、このADLS Gen2 [ドキュメントを参照](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-storage)。

## ADLS Gen2アカウントの接続

必要な資格情報を収集したら、次の手順に従って新しい受信ベース接続を作成し、ADLS Gen2アカウントをプラットフォームにリンクします。

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし</a> 、左のナビゲーションバーから「 **Sources** 」を選択して、ソースワークスペースにアク *セスします* 。 「カタ *ログ* 」タブには、受信ベース接続の作成に使用できる様々なソースが表示されます。 各ソースには、関連付けられた既存のベース接続の数が表示されます。

クラウ *ドストレージ* カテゴリの下で、「 **Azure Data Lake Gen2** 」を選択し、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、そのドキュメントのソース表示に接続するオプションが表示されます。 新しい受信ベース接続を作成するには、[ソースの接続]を **クリックしま**&#x200B;す。

![](../../../../images/tutorials/create/adls-gen2/catalog.png)

[ *Azure Data Lake Gen2に接続* ]ダイアログが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「新規アカウント」 **を選択しま**&#x200B;す。 表示される入力フォームで、ベース接続に名前、オプションの説明、およびADLS Gen2資格情報を指定します。 終了したら、[接続 **** ]を選択し、新しいベース接続が確立されるまでの時間を設定します。

![](../../../../images/tutorials/create/adls-gen2/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するADLS Gen2アカウントを選択し、「次へ」を選択 **して** 、先に進みます。

![](../../../../images/tutorials/create/adls-gen2/existing.png)

## 次の手順

このチュートリアルに従って、ADLS Gen2アカウントへのベース接続を確立しました。 次のチュートリアルに進み、データフローを設定して、 [クラウドストレージのデータをプラットフォームに取り込むことができます](../../dataflow/cloud-storage.md)。