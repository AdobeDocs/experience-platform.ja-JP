---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure Blob;azure blob;Azure BLOBコネクタ
solution: Experience Platform
title: UIでAzure Blob Source Connectionを作成する
topic-legacy: overview
type: Tutorial
description: プラットフォームユーザーインターフェイスを使用してAzure BLOBソースコネクタを作成する方法を説明します。
exl-id: 0e54569b-7305-4065-981e-951623717648
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '644'
ht-degree: 7%

---

# UIに[!DNL Azure Blob]ソース接続を作成する

このチュートリアルでは、プラットフォームのユーザーインターフェイスを使用して[!DNL Azure Blob]（以下「[!DNL Blob]」と呼びます）を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効な[!DNL Blob]接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/batch/cloud-storage.md)のチュートリアルに進むことができます。

### サポートされているファイル形式

[!DNL Experience Platform] は、外部ストレージから取り込む次のファイル形式をサポートしています。

- 区切り文字区切り値(DSV):DSV形式のデータ・ファイルのサポートは、現在、コンマ区切り値に制限されています。 DSV形式のファイル内のフィールド・ヘッダーの値は、英数字とアンダースコアのみで構成する必要があります。 一般的なDSVファイルは、今後サポートされる予定です。
- JavaScript Object Notation (JSON):JSON形式のデータファイルは、XDMに準拠している必要があります。
- Apacheパーケット：パーケット形式のデータファイルは、XDMに準拠している必要があります。

### 必要な資格情報の収集

プラットフォームの[!DNL Blob]ストレージにアクセスするには、次の資格情報の有効な値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | Experience Platformに対する[!DNL Blob]の認証に必要な認証情報を含む文字列です。 [!DNL Blob]接続文字列パターンは次のとおりです。`DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`. 接続文字列の詳細については、[接続文字列の設定](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string)に関するこの[!DNL Blob]ドキュメントを参照してください。 |
| `sasUri` | [!DNL Blob]アカウントに接続するための代替認証タイプとして使用できる共有アクセス署名URIです。 [!DNL Blob] SAS URIパターンは次のとおりです。`https://{ACCOUNT_NAME}.blob.core.windows.net/?sv=<storage version>&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>`詳しくは、[共有アクセス署名URI](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication)のこの[!DNL Blob]ドキュメントを参照してください。 |

## [!DNL Blob]アカウントに接続

必要な資格情報を収集したら、次の手順に従って[!DNL Blob]アカウントをプラットフォームにリンクできます。

[プラットフォームUI](https://platform.adobe.com)で、左のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索バーを使用して、使用する特定のソースを見つけることもできます。

[!UICONTROL クラウドストレージ]カテゴリの下で、**[!UICONTROL Azure Blobストレージ]**&#x200B;を選択し、**[!UICONTROL 追加data]**&#x200B;を選択します。

![カタログ](../../../../images/tutorials/create/blob/catalog.png)

**[!UICONTROL Azure Blobストレージ]**&#x200B;に接続ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する[!DNL Blob]アカウントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![既存の](../../../../images/tutorials/create/blob/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、新しい[!DNL Blob]アカウントの名前とオプションの説明を入力します。

**接続文字列を使用して認証する**

[!DNL Blob]コネクタは、アクセス用に異なる認証タイプを提供します。 「[!UICONTROL アカウント認証]」で、「**[!UICONTROL ConnectionString]**」を選択して、接続文字列ベースの秘密鍵証明書を使用します。

![接続文字列](../../../../images/tutorials/create/blob/connectionstring.png)

**共有アクセス署名URIを使用して認証する**

共有アクセス署名(SAS)URIを使用すると、[!DNL Blob]アカウントに対する安全な委任承認を許可できます。 SASベースの認証では、権限、開始、有効期限の設定、特定のリソースに対する規定を行えるため、SASを使用して、様々なアクセス度の認証資格情報を作成できます。

**[!UICONTROL SasURIAuthentication]**&#x200B;を選択し、[!DNL Blob] SAS URIを指定します。 「**[!UICONTROL ソースに接続]**」を選択して続行します。

![sas-uri](../../../../images/tutorials/create/blob/sas-uri.png)

## 次の手順

このチュートリアルに従うと、[!DNL Blob]アカウントへの接続が確立されます。 次のチュートリアルに進み、[データフローを設定して、クラウドストレージのデータをプラットフォーム](../../dataflow/batch/cloud-storage.md)に取り込むことができます。
