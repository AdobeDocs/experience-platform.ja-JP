---
title: UI での Azure Blob ソース接続の作成
description: Platform ユーザーインターフェイスを使用して Azure BLOB ソースコネクタを作成する方法を説明します。
exl-id: 0e54569b-7305-4065-981e-951623717648
source-git-commit: d22c71fb77655c401f4a336e339aaf8b3125d1b6
workflow-type: tm+mt
source-wordcount: '827'
ht-degree: 27%

---

# の作成 [!DNL Azure Blob] UI のソース接続

このチュートリアルでは、 [!DNL Azure Blob] （以下「」という。）[!DNL Blob]&quot;) Platform ユーザーインターフェイスを使用したソース接続。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：顧客体験データを整理するための標準化されたフレームワークです。Experience Platform。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Blob] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/batch/cloud-storage.md)に関するチュートリアルに進むことができます。

### サポートされているファイル形式

Experience Platformは、次のファイル形式をサポートしており、外部ストレージから取り込むことができます。

* 区切り文字区切り値 (DSV)：タブ、コンマ、パイプ、セミコロン、ハッシュなどの任意の 1 列区切り文字を使用して、任意の形式のフラット・ファイルを収集できます。
* JavaScript オブジェクト表記法 (JSON):JSON 形式のデータファイルは、XDM に準拠している必要があります。
* Apache Parquet: Parquet 形式のデータファイルは、XDM に準拠している必要があります。

### 必要な資格情報の収集

次の項目にアクセスするには、 [!DNL Blob] Experience Platform上のストレージの場合は、次の資格情報に有効な値を指定する必要があります。

>[!BEGINTABS]

>[!TAB 接続文字列認証]

| 資格情報 | 説明 |
| --- | --- |
| 接続文字列 | 認証に必要な認証情報を含む文字列 [!DNL Blob] をExperience Platformに追加します。 The [!DNL Blob] 接続文字列のパターンは次のとおりです。 `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`. 接続文字列について詳しくは、 [!DNL Blob] ～に関する文書 [接続文字列の設定](https://learn.microsoft.com/ja-jp/azure/storage/common/storage-configure-connection-string). |

>[!TAB SAS URI 認証]

| 資格情報 | 説明 |
| --- | --- |
| SAS URI | 代替の認証タイプとして使用して、 [!DNL Blob] アカウント。 The [!DNL Blob] SAS URI パターンは次のとおりです。 `https://{ACCOUNT_NAME}.blob.core.windows.net/?sv=<storage version>&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>` 詳しくは、 [!DNL Blob] ～に関する文書 [共有アクセス署名 URI](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication). |
| コンテナ | アクセスを指定するコンテナの名前です。 を使用して新しいアカウントを作成する場合、 [!DNL Blob] ソースとして指定する場合、選択したサブフォルダーへのユーザーアクセスを指定するコンテナ名を指定できます。 |
| フォルダーパス | アクセス権を付与するフォルダーのパスです。 |

>[!ENDTABS]

必要な資格情報を収集したら、次の手順に従って、 [!DNL Blob] ストレージからExperience Platformへ

## [!DNL Blob] アカウントの接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索バーを使用して、利用したい特定のソースを見つけることもできます。

の下 [!UICONTROL クラウドストレージ] カテゴリ、選択 **[!UICONTROL Azure Blob ストレージ]**&#x200B;を選択し、 **[!UICONTROL データを追加]**.

![Experience Platformソースカタログで Azure BLOB ストレージソースが選択されています。](../../../../images/tutorials/create/blob/catalog.png)

The **[!UICONTROL Azure Blob ストレージに接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL Blob] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/blob/existing.png)

### 新しいアカウント

>[!TIP]
>
>作成後は、 [!DNL Blob] ベース接続。 認証タイプを変更するには、新しいベース接続を作成する必要があります。

新しいアカウントを作成する場合は、「 **[!UICONTROL 新しいアカウント]**&#x200B;をクリックし、新しい [!DNL Blob] アカウント。

![Azure BLOB ストレージソースの新しいアカウント画面。](../../../../images/tutorials/create/blob/new.png)

The [!DNL Blob] ソースは、アカウントキー認証と共有アクセス署名 (SAS) 認証の両方をサポートしています。 アカウントキーベースの認証では検証に接続文字列が必要ですが、SAS 認証では、アカウントの安全な委任認証を可能にする URI を使用します。

この手順では、コンテナの名前とサブフォルダーのパスを定義して、アカウントがアクセスできるサブフォルダーを指定することもできます。

>[!BEGINTABS]

>[!TAB 接続文字列]

アカウントキーを認証するには、「 」を選択します。 **[!UICONTROL アカウントキー認証]** 接続文字列を指定します。 この手順の間に、アクセス先のサブフォルダーのコンテナ名とパスを指定することもできます。 終了したら、「 」を選択します。 **[!UICONTROL ソースに接続]**.

![接続文字列](../../../../images/tutorials/create/blob/connectionstring.png)

>[!TAB SAS URI]

SAS ベースの認証では、権限設定、開始日、有効期限日、および特定のリソースに対するプロビジョニングを設定できるので、SAS を使用して、様々なアクセス度の認証資格情報を作成できます。

共有アクセス署名を使用して認証するには、「 」を選択します。 **[!UICONTROL 共有アクセス署名認証]** SAS URI を指定します。 この手順の間に、アクセス先のサブフォルダーのコンテナ名とパスを指定することもできます。 終了したら、「 」を選択します。 **[!UICONTROL ソースに接続]**.

![sas-uri](../../../../images/tutorials/create/blob/sas-uri.png)

>[!ENDTABS]

## 次の手順

このチュートリアルでは、[!DNL Blob] アカウントとの接続を確立しました。次のチュートリアルに進み、 [データフローを設定して、クラウドストレージから Platform にデータを取り込みます。](../../dataflow/batch/cloud-storage.md).
