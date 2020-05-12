---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIにAzure Data LakeストレージGen2ソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 799445eca080175e2bffc49c6714f0c812b9bbea
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 0%

---


# UIにAzure Data LakeストレージGen2ソースコネクタを作成する

Adobe Experience Platformのソースコネクターは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、Azure Data LakeストレージGen2 （以下「ADLS Gen2」と呼ばれる）ソースコネクタをプラットフォームユーザーインターフェイスを使用して認証する手順を説明します。

## はじめに

このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

- [Experience Data Model(XDM)System](../../../../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成における主な原則とベストプラクティスが含まれます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既にADLS Gen2ベース接続をお持ちの場合は、このドキュメントの残りの部分をスキップし、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/batch/cloud-storage.md)。

### 必要な資格情報の収集

ADLS Gen2ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `url` | ADLS Gen2のエンドポイントです。 |
| `servicePrincipalId` | アプリケーションのクライアントID。 |
| `servicePrincipalKey` | アプリのキー。 |
| `tenant` | アプリケーションを含むテナント情報。 |

これらの値の詳細については、 [このADLS Gen2ドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-storage)。

## ADLS Gen2アカウントの接続

必要な資格情報を収集したら、次の手順に従って新しい受信ベース接続を作成し、ADLS Gen2アカウントをプラットフォームにリンクします。

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし、左のナビゲーションバーで「</a> Sources **** 」を選択して *Sources* ワークスペースにアクセスします。 「 *カタログ* 」タブには様々なソースが表示され、このソースを使用して受信ベース接続を作成できます。 各ソースには、関連付けられた既存のベース接続の数が表示されます。

[ *クラウドストレージ* ]カテゴリの下で、[ **Azure Data Lake Gen2** ]を選択して、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ソース表示のドキュメントに接続するためのオプションが表示されます。 新しい受信ベース接続を作成するには、[ **接続元**]をクリックします。

![](../../../../images/tutorials/create/adls-gen2/catalog.png)

[ *Azure Data Lake Gen2に* 接続]ダイアログが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **新規アカウント**」を選択します。 表示される入力フォームで、基本接続に名前、オプションの説明、およびADLS Gen2資格情報を指定します。 終了したら、[ **接続** ]を選択し、新しいベース接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/adls-gen2/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するADLS Gen2アカウントを選択し、「 **次へ** 」を選択して次に進みます。

![](../../../../images/tutorials/create/adls-gen2/existing.png)

## 次の手順

このチュートリアルに従って、ADLS Gen2アカウントへの基本接続を確立しました。 次のチュートリアルに進み、クラウドストレージのデータをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/batch/cloud-storage.md)。