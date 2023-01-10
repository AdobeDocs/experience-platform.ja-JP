---
keywords: Experience Platform；ホーム；人気の高いトピック；Azure Blob;Azure Blob;Azure Blob コネクタ
solution: Experience Platform
title: UI での Azure Blob ソース接続の作成
type: Tutorial
description: Platform ユーザーインターフェイスを使用して Azure BLOB ソースコネクタを作成する方法を説明します。
exl-id: 0e54569b-7305-4065-981e-951623717648
source-git-commit: ed92bdcd965dc13ab83649aad87eddf53f7afd60
workflow-type: tm+mt
source-wordcount: '644'
ht-degree: 39%

---

# の作成 [!DNL Azure Blob] UI のソース接続

このチュートリアルでは、 [!DNL Azure Blob] （以下「」という。）[!DNL Blob]」) を使用して、Platform ユーザーインターフェイスを使用します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Blob] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/batch/cloud-storage.md)に関するチュートリアルに進むことができます。

### サポートされているファイル形式

[!DNL Experience Platform] は、次のファイル形式を外部ストレージから取り込むことができます。

- 区切り文字区切り値 (DSV):DSV 形式のデータ・ファイルのサポートは、現在、コンマ区切りの値に制限されています。 DSV フォーマット・ファイル内のフィールド・ヘッダーの値は、英数字とアンダースコアのみで構成する必要があります。 一般的な DSV ファイルのサポートは、今後提供される予定です。
- JavaScript オブジェクト表記 (JSON):JSON 形式のデータファイルは、XDM に準拠している必要があります。
- Apache Parquet:Parquet 形式のデータファイルは、XDM に準拠している必要があります。

### 必要な資格情報の収集

次の項目にアクセスするには、 [!DNL Blob] Platform 上のストレージの場合、次の資格情報の有効な値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | 認証に必要な認証情報を含む文字列 [!DNL Blob] Experience Platformに この [!DNL Blob] 接続文字列のパターン： `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`. 接続文字列について詳しくは、 [!DNL Blob] 文書 [接続文字列の設定](https://learn.microsoft.com/ja-jp/azure/storage/common/storage-configure-connection-string). |
| `sasUri` | 代替の認証タイプとして使用して、 [!DNL Blob] アカウント この [!DNL Blob] SAS URI パターンは次のとおりです。 `https://{ACCOUNT_NAME}.blob.core.windows.net/?sv=<storage version>&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>` 詳しくは、 [!DNL Blob] 文書 [共有アクセス署名 URI](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication). |

## [!DNL Blob] アカウントを接続

必要な資格情報を収集したら、以下の手順に従って [!DNL Blob] アカウントを Platform にリンクできます。

[Platform UI](https://platform.adobe.com) で、左側のナビゲーションバーから&#x200B;**[!UICONTROL ソース]**&#x200B;を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索バーを使用して、利用したい特定のソースを見つけることもできます。

以下 [!UICONTROL クラウドストレージ] カテゴリ、選択 **[!UICONTROL Azure Blob ストレージ]**&#x200B;を選択し、 **[!UICONTROL データを追加]**.

![カタログ](../../../../images/tutorials/create/blob/catalog.png)

この **[!UICONTROL Azure Blob ストレージに接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL Blob] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/blob/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「 **[!UICONTROL 新しいアカウント]**&#x200B;新しい [!DNL Blob] アカウント

**接続文字列を使用した認証**

この [!DNL Blob] コネクタは、アクセス用に様々な認証タイプを提供します。 の下 [!UICONTROL アカウント認証] 選択 **[!UICONTROL ConnectionString]** 接続文字列ベースの資格情報を使用するには、次の手順を実行します。

![接続文字列](../../../../images/tutorials/create/blob/connectionstring.png)

**共有アクセス署名 URI を使用した認証**

共有アクセス署名 (SAS)URI を使用すると、 [!DNL Blob] アカウント SAS ベースの認証では、権限設定、開始日、有効期限日、および特定のリソースに対するプロビジョニングを設定できるので、SAS を使用して、様々なアクセス度の認証資格情報を作成できます。

選択 **[!UICONTROL SasURIAuthentication]** 次に、 [!DNL Blob] SAS URI。 選択 **[!UICONTROL ソースに接続]** をクリックして続行します。

![sas-uri](../../../../images/tutorials/create/blob/sas-uri.png)

## 次の手順

このチュートリアルでは、[!DNL Blob] アカウントとの接続を確立しました。次のチュートリアルに進み、 [データフローを設定して、クラウドストレージから Platform にデータを取り込みます。](../../dataflow/batch/cloud-storage.md).
