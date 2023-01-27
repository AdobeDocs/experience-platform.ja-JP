---
title: UI でのGoogle Cloud ストレージソース接続の作成
description: Adobe Experience Platform UI を使用してGoogle Cloud Storage ソース接続を作成する方法を説明します。
exl-id: 3258ccd7-757c-4c4a-b7bb-0e8c9de3b50a
source-git-commit: 7181cb92dd44d8005fe1054020ffeb36c309b42e
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 46%

---

# UI での [!DNL Google Cloud Storage] ソース接続の作成

このチュートリアルでは、 [!DNL Google Cloud Storage] Adobe Experience Platform UI を使用したソース接続

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Google Cloud Storage] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/batch/cloud-storage.md)に関するチュートリアルに進むことができます。

### サポートされているファイル形式

[!DNL Experience Platform] は、次のファイル形式を外部ストレージから取り込むことができます。

* 区切り文字区切り値 (DSV):DSV 形式のデータ・ファイルの区切り文字として、任意の 1 文字の値を使用できます。
* JavaScript オブジェクト表記 (JSON):JSON 形式のデータファイルは、XDM に準拠している必要があります。
* Apache Parquet:Parquet 形式のデータファイルは、XDM に準拠している必要があります。

### 必要な資格情報の収集

次の項目にアクセスするには、 [!DNL Google Cloud Storage] Platform のデータには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| アクセスキー ID | 61 文字の英数字から成る文字列で、 [!DNL Google Cloud Storage] アカウントを Platform に送信します。 |
| 秘密アクセスキー | ユーザーの認証に使用される 40 文字のベース 64 エンコードされた文字列 [!DNL Google Cloud Storage] アカウントを Platform に送信します。 |
| バケット名 | お客様の [!DNL Google Cloud Storage] バケット。 クラウドストレージ内の特定のサブフォルダーへのアクセスを提供する場合は、バケット名を指定する必要があります。 |
| フォルダーパス | アクセス権を付与するフォルダーのパスです。 |

これらの値について詳しくは、[Google Cloud Storage の HMAC キー](https://cloud.google.com/storage/docs/authentication/hmackeys#overview)ガイドを参照してください。独自のアクセスキー ID と秘密アクセスキーを生成する手順については、 [[!DNL Google Cloud Storage] 概要](../../../../connectors/cloud-storage/google-cloud-storage.md).

必要な資格情報を収集したら、以下の手順に従って [!DNL Google Cloud Storage] アカウントを Platform にリンクできます。

## [!DNL Google Cloud Storage] アカウントを接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 [!UICONTROL クラウドストレージ] カテゴリ、選択 **[!UICONTROL Google Cloud Storage]** 次に、 **[!UICONTROL データを追加]**.

![ソースカタログページが表示される Platform UI 画面。](../../../../images/tutorials/create/google-cloud-storage/catalog.png)

この **[!UICONTROL Google Cloud Storage に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL Google Cloud Storage] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![Google Cloud Storage ソースの既存のアカウントページが表示される Platform UI 画面](../../../../images/tutorials/create/google-cloud-storage/existing.png)

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）および [!DNL Google Cloud Storage] 資格情報。 この手順では、サブフォルダーのパス名を定義して、アカウントがアクセスするサブフォルダーを指定することもできます。

終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![Google Cloud Storage ソースの新しいアカウントページが表示される Platform UI 画面。](../../../../images/tutorials/create/google-cloud-storage/new.png)


## 次の手順

このチュートリアルでは、[!DNL Google Cloud Storage] アカウントとの接続を確立しました。次のチュートリアルに進み、 [データフローを設定して、クラウドストレージから Platform にデータを取り込みます。](../../dataflow/batch/cloud-storage.md).
