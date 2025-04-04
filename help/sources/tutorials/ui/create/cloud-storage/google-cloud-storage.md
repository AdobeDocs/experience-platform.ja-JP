---
title: UI でのGoogle Cloud Storage Source接続の作成
description: Adobe Experience Platform UI を使用して、Google Cloud Storage のソース接続を作成する方法について説明します。
exl-id: 3258ccd7-757c-4c4a-b7bb-0e8c9de3b50a
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 40%

---

# UI での [!DNL Google Cloud Storage] ソース接続の作成

このチュートリアルでは、Adobe Experience Platform UI を使用して [!DNL Google Cloud Storage] ソース接続を作成する手順について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Google Cloud Storage] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/batch/cloud-storage.md)に関するチュートリアルに進むことができます。

### サポートされているファイル形式

[!DNL Experience Platform] では、外部ストレージから取り込む次のファイル形式をサポートしています。

* 区切り文字区切り値（DSV）：任意の単一文字の値を、DSV 形式のデータファイルの区切り文字として使用できます。
* JavaScript Object Notation （JSON）: JSON 形式のデータファイルは XDM に準拠している必要があります。
* Apache Parquet:Parquet 形式のデータファイルは XDM に準拠している必要があります。

### 必要な資格情報の収集

Experience Platformで [!DNL Google Cloud Storage] データにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| アクセスキー ID | [!DNL Google Cloud Storage] アカウントをExperience Platformに認証するために使用される 61 文字の英数字の文字列。 |
| シークレットアクセスキー | [!DNL Google Cloud Storage] アカウントをExperience Platformに認証するために使用される 40 文字の base-64 エンコード文字列。 |
| バケット名 | [!DNL Google Cloud Storage] バケットの名前。 クラウドストレージ内の特定のサブフォルダーへのアクセスを提供する場合は、バケット名を指定する必要があります。 |
| フォルダーパス | アクセス権を付与するフォルダーへのパス。 |

これらの値について詳しくは、[Google Cloud Storage の HMAC キー](https://cloud.google.com/storage/docs/authentication/hmackeys#overview)ガイドを参照してください。独自のアクセスキー ID と秘密アクセスキーを生成する手順については、[[!DNL Google Cloud Storage]  概要 ](../../../../connectors/cloud-storage/google-cloud-storage.md) を参照してください。

必要な資格情報を収集したら、次の手順に従って [!DNL Google Cloud Storage] アカウントをExperience Platformにリンクできます。

## [!DNL Google Cloud Storage] アカウントを接続

Experience Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、「[!UICONTROL  ソース ] ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

[!UICONTROL  クラウドストレージ ] カテゴリで、「**[!UICONTROL Google クラウドストレージ]**」を選択し、次に「**[!UICONTROL データを追加]**」を選択します。

![ ソースカタログページを表示しているExperience Platform UI 画面 ](../../../../images/tutorials/create/google-cloud-storage/catalog.png)

**[!UICONTROL Google クラウドストレージに接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Google Cloud Storage] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![Google Cloud Storage ソースの既存のアカウントページを表示しているExperience Platform UI 画面 ](../../../../images/tutorials/create/google-cloud-storage/existing.png)

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、[!DNL Google Cloud Storage] 資格情報を入力します。 この手順では、サブフォルダーへのパス名を定義することで、アカウントがアクセスできるサブフォルダーを指定することもできます。

終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![Google Cloud Storage ソースの新しいアカウントページを表示しているExperience Platform UI 画面。](../../../../images/tutorials/create/google-cloud-storage/new.png)


## 次の手順

このチュートリアルでは、[!DNL Google Cloud Storage] アカウントとの接続を確立しました。次のチュートリアルに進み、[ データフローを設定して、クラウドストレージからExperience Platformにデータを取り込む ](../../dataflow/batch/cloud-storage.md) ことができます。
