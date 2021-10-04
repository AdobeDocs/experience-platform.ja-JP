---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure Blob;azure BLOB;Azure BLOB コネクタ
solution: Experience Platform
title: UI での Azure BLOB ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Platform ユーザーインターフェイスを使用して Azure BLOB ソースコネクタを作成する方法を説明します。
exl-id: 0e54569b-7305-4065-981e-951623717648
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '644'
ht-degree: 9%

---

# UI での [!DNL Azure Blob] ソース接続の作成

このチュートリアルでは、Platform のユーザーインターフェイスを使用して [!DNL Azure Blob]（以下「[!DNL Blob]」と呼びます）を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを整理する際に使用する標準化されたExperience Platformフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターの UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Blob] 接続がある場合は、このドキュメントの残りの部分をスキップし、[ データフローの設定 ](../../dataflow/batch/cloud-storage.md) に関するチュートリアルに進んでください。

### サポートされているファイル形式

[!DNL Experience Platform] は、次のファイル形式をサポートし、外部ストレージから取り込むことができます。

- 区切り文字区切り値 (DSV):DSV 形式のデータ・ファイルのサポートは、現在、コンマ区切り値に制限されています。 DSV フォーマット・ファイル内のフィールド・ヘッダーの値は、英数字とアンダースコアのみで構成する必要があります。 一般的な DSV ファイルのサポートは、今後提供される予定です。
- JavaScript オブジェクト表記 (JSON):JSON 形式のデータファイルは、XDM に準拠している必要があります。
- Apache Parquet:Parquet 形式のデータファイルは、XDM に準拠している必要があります。

### 必要な資格情報の収集

Platform の [!DNL Blob] ストレージにアクセスするには、次の資格情報の有効な値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL Blob] の認証に必要な認証情報を含むExperience Platform。 [!DNL Blob] 接続文字列パターンは次のとおりです。`DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`. 接続文字列の詳細については、[ 接続文字列 ](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string) の設定に関するこの [!DNL Blob] ドキュメントを参照してください。 |
| `sasUri` | [!DNL Blob] アカウントに接続するための代替の認証タイプとして使用できる共有アクセス署名 URI です。 [!DNL Blob] SAS URI パターンは次のとおりです。`https://{ACCOUNT_NAME}.blob.core.windows.net/?sv=<storage version>&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>` 詳しくは、[ 共有アクセスの署名 URI](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication) にあるこの [!DNL Blob] ドキュメントを参照してください。 |

## [!DNL Blob] アカウントに接続

必要な資格情報を収集したら、以下の手順に従って [!DNL Blob] アカウントを Platform にリンクできます。

[Platform UI](https://platform.adobe.com) で、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、「[!UICONTROL  ソース ]」ワークスペースにアクセスします。 [!UICONTROL  カタログ ] 画面には、アカウントを作成するための様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索バーを使用して、作業対象の特定のソースを見つけることもできます。

「[!UICONTROL  クラウドストレージ ]」カテゴリで、「**[!UICONTROL Azure BLOB ストレージ]**」を選択し、「**[!UICONTROL データを追加]**」を選択します。

![カタログ](../../../../images/tutorials/create/blob/catalog.png)

「**[!UICONTROL Azure BLOB ストレージに接続]**」ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL Blob] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して次に進みます。

![既存](../../../../images/tutorials/create/blob/existing.png)

### 新規アカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、新しい [!DNL Blob] アカウントの名前と説明を入力します。

**接続文字列を使用した認証**

[!DNL Blob] コネクタは、アクセス用に異なる認証タイプを提供します。 「[!UICONTROL  アカウント認証 ]」で、「**[!UICONTROL ConnectionString]**」を選択して、接続文字列ベースの資格情報を使用します。

![接続文字列](../../../../images/tutorials/create/blob/connectionstring.png)

**共有アクセス署名 URI を使用した認証**

共有アクセス署名 (SAS) URI を使用すると、[!DNL Blob] アカウントに対する安全な委任承認を実行できます。 SAS ベースの認証では、権限、開始日、有効期限、および特定のリソースに対するプロビジョニングを設定できるので、SAS を使用して、様々なアクセスレベルで認証資格情報を作成できます。

**[!UICONTROL SasURIAuthentication]** を選択し、[!DNL Blob] SAS URI を指定します。 「**[!UICONTROL ソースに接続]**」を選択して続行します。

![sas-uri](../../../../images/tutorials/create/blob/sas-uri.png)

## 次の手順

このチュートリアルに従って、[!DNL Blob] アカウントへの接続を確立しました。 次のチュートリアルに進み、クラウドストレージから Platform](../../dataflow/batch/cloud-storage.md) にデータを取り込むように [ データフローを設定します。
