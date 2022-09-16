---
keywords: Experience Platform；ホーム；人気の高いトピック；Google Cloud Storage;google クラウドストレージ；GCS;gcs
solution: Experience Platform
title: UI でのGoogle Cloud ストレージソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用してGoogle Cloud Storage ソース接続を作成する方法を説明します。
exl-id: 3258ccd7-757c-4c4a-b7bb-0e8c9de3b50a
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '594'
ht-degree: 28%

---

# UI での [!DNL Google Cloud Storage] ソース接続の作成

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、 [!DNL Google Cloud Storage] （以下「GCS」という） [!DNL Platform] ユーザーインターフェイス。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):Experience Platformが顧客体験データを整理する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な GCS 接続がある場合は、このドキュメントの残りの部分をスキップして、次のチュートリアルに進んでください。 [データフローの設定](../../dataflow/batch/cloud-storage.md).

### サポートされているファイル形式

[!DNL Experience Platform] は、次のファイル形式を外部ストレージから取り込むことができます。

* 区切り文字区切り値 (DSV):DSV 形式のデータ・ファイルのサポートは、現在、コンマ区切りの値に制限されています。 DSV フォーマット・ファイル内のフィールド・ヘッダーの値は、英数字とアンダースコアのみで構成する必要があります。 一般的な DSV ファイルのサポートは、今後提供される予定です。
* JavaScript オブジェクト表記 (JSON):JSON 形式のデータファイルは、XDM に準拠している必要があります。
* Apache Parquet:Parquet 形式のデータファイルは、XDM に準拠している必要があります。

### 必要な認証情報の収集

で GCS データにアクセスするには、以下を実行します。 [!DNL Platform]に値を指定する場合は、次の値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| アクセスキー ID | 61 文字の英数字から成る文字列で、 [!DNL Google Cloud Storage] アカウントを Platform に送信します。 |
| 秘密アクセスキー | ユーザーの認証に使用される 40 文字のベース 64 エンコードされた文字列 [!DNL Google Cloud Storage] アカウントを Platform に送信します。 |

これらの値について詳しくは、 [Google Cloud Storage の HMAC キー](https://cloud.google.com/storage/docs/authentication/hmackeys#overview) ガイド。 独自のアクセスキー ID と秘密アクセスキーを生成する手順については、 [[!DNL Google Cloud Storage] 概要](../../../../connectors/cloud-storage/google-cloud-storage.md).

## [!DNL Google Cloud Storage] アカウントを接続

必要な資格情報を収集したら、以下の手順に従って、GCS アカウントをにリンクできます。 [!DNL Platform].

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL データベース]** カテゴリ、選択 **[!UICONTROL Google Cloud Storage]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** をクリックして、新しい GCS コネクタを作成します。

![カタログ](../../../../images/tutorials/create/google-cloud-storage/catalog.png)

この **[!UICONTROL Google Cloud Storage に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「 **[!UICONTROL 新しいアカウント]**. 表示される入力フォームで、名前、説明（オプション）および GCS 資格情報を入力します。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![接続](../../../../images/tutorials/create/google-cloud-storage/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する GCS アカウントを選択し、「 」を選択します **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/google-cloud-storage/existing.png)

## 次の手順

このチュートリアルに従って、GCS アカウントへの接続を確立しました。 次のチュートリアルに進み、 [データフローを設定して、クラウドストレージからにデータを取り込みます。 [!DNL Platform]](../../dataflow/batch/cloud-storage.md).
