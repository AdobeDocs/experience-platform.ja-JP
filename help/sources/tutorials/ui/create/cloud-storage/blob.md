---
title: UI での Azure Blob Source接続の作成
description: Experience Platform ユーザーインターフェイスを使用して、Azure Blob ソースコネクタを作成する方法を説明します。
exl-id: 0e54569b-7305-4065-981e-951623717648
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '819'
ht-degree: 25%

---

# UI での [!DNL Azure Blob] ソース接続の作成

このチュートリアルでは、Experience Platform ユーザーインターフェイスを使用して、[!DNL Azure Blob] （以下「[!DNL Blob]」と呼びます）のソース接続を作成する手順について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム ](../../../../../xdm/home.md):Experience Platformでカスタマーエクスペリエンスデータを整理するための標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Blob] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/batch/cloud-storage.md)に関するチュートリアルに進むことができます。

### サポートされているファイル形式

Experience Platformでは、外部ストレージから取り込む次のファイル形式をサポートしています。

* 区切り文字区切り値（DSV）: タブ、コンマ、パイプ、セミコロン、ハッシュなど、任意の単一列の区切り文字を使用して、任意の形式のフラットファイルを収集できます。
* JavaScript Object Notation （JSON）: JSON 形式のデータファイルは XDM に準拠している必要があります。
* Apache Parquet:Parquet 形式のデータファイルは XDM に準拠している必要があります。

### 必要な資格情報の収集

Experience Platformの [!DNL Blob] ストレージにアクセスするには、次の資格情報に対する有効な値を指定する必要があります。

>[!BEGINTABS]

>[!TAB  接続文字列認証 ]

| 資格情報 | 説明 |
| --- | --- |
| 接続文字列 | Experience Platformに対する [!DNL Blob] の認証に必要な認証情報を含む文字列。 [!DNL Blob] の接続文字列パターンは `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}` です。 接続文字列について詳しくは、この [!DNL Blob] ドキュメント [ 接続文字列の設定 ](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string) を参照してください。 |

>[!TAB SAS URI 認証 ]

| 資格情報 | 説明 |
| --- | --- |
| SAS URI | [!DNL Blob] アカウントを接続するための代替認証タイプとして使用できる共有アクセス署名 URI です。 [!DNL Blob] の SAS URI パターンを以下に示します。`https://{ACCOUNT_NAME}.blob.core.windows.net/?sv=<storage version>&st={START_TIME}&se={EXPIRE_TIME}&sr={RESOURCE}&sp={PERMISSIONS}>&sip=<{IP_RANGE}>&spr={PROTOCOL}&sig={SIGNATURE}>` 詳しくは、この [!DNL Blob] ドキュメントの [ 共有アクセス署名 URI](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication) を参照してください。 |
| コンテナ | アクセスを指定するコンテナの名前。 [!DNL Blob] ソースで新しいアカウントを作成する際に、コンテナ名を指定して、選択したサブフォルダーへのユーザーアクセスを指定できます。 |
| フォルダーパス | アクセス権を付与するフォルダーへのパス。 |

>[!ENDTABS]

必要な資格情報を収集したら、次の手順に従って [!DNL Blob] ストレージをExperience Platformに接続できます

## [!DNL Blob] アカウントを接続

Experience Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、「[!UICONTROL  ソース ] ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索バーを使用して、利用したい特定のソースを見つけることもできます。

[!UICONTROL  クラウドストレージ ] カテゴリで、「**[!UICONTROL Azure Blob ストレージ]**」を選択し、次に「**[!UICONTROL データを追加]**」を選択します。

![Experience Platform ソースカタログと Azure Blob ストレージソースが選択されています。](../../../../images/tutorials/create/blob/catalog.png)

**[!UICONTROL Azure Blob Storage に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL Blob] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/blob/existing.png)

### 新しいアカウント

>[!TIP]
>
>作成した後は、[!DNL Blob] ベース接続の認証タイプを変更できません。 認証タイプを変更するには、新しいベース接続を作成する必要があります。

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、新しい [!DNL Blob] アカウントの名前と説明（オプション）を入力します。

![Azure Blob ストレージソースの新しいアカウント画面 ](../../../../images/tutorials/create/blob/new.png)

[!DNL Blob] ソースは、アカウントキー認証と共有アクセス署名（SAS）認証の両方をサポートしています。 アカウントキーベースの認証では、検証用の接続文字列が必要ですが、SAS 認証では、アカウントの安全なデリゲート承認を可能にする URI を利用します。

この手順では、コンテナの名前とサブフォルダーへのパスを定義することで、アカウントがアクセスできるサブフォルダーを指定することもできます。

>[!BEGINTABS]

>[!TAB  接続文字列 ]

アカウントキーを使用して認証するには、「**[!UICONTROL アカウントキー認証]**」を選択し、接続文字列を指定します。 この手順では、アクセス先のサブフォルダーのコンテナ名およびパスを指定することもできます。 終了したら「**[!UICONTROL ソースに接続]**」を選択します。

![ 接続文字列 ](../../../../images/tutorials/create/blob/connectionstring.png)

>[!TAB SAS URI]

SAS ベースの認証では、権限、開始日、有効期限および特定のリソースへのプロビジョニングを設定できるので、SAS を使用して、様々なレベルのアクセスで認証資格情報を作成できます。

共有アクセス署名を使用して認証するには、「**[!UICONTROL 共有アクセス署名認証]**」を選択し、SAS URI を指定します。 この手順では、アクセス先のサブフォルダーのコンテナ名およびパスを指定することもできます。 終了したら「**[!UICONTROL ソースに接続]**」を選択します。

![sas-uri](../../../../images/tutorials/create/blob/sas-uri.png)

>[!ENDTABS]

## 次の手順

このチュートリアルでは、[!DNL Blob] アカウントとの接続を確立しました。次のチュートリアルに進み、[ データフローを設定して、クラウドストレージからExperience Platformにデータを取り込む ](../../dataflow/batch/cloud-storage.md) ことができます。
