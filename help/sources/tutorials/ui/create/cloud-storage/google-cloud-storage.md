---
keywords: Experience Platform；ホーム；人気のあるトピック；Google Cloud Storage;google クラウドストレージ；GCS;gcs
solution: Experience Platform
title: UI でのGoogle Cloud ストレージソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用してGoogle Cloud Storage ソース接続を作成する方法を説明します。
exl-id: 3258ccd7-757c-4c4a-b7bb-0e8c9de3b50a
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '594'
ht-degree: 10%

---

# UI での [!DNL Google Cloud Storage] ソース接続の作成

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに従って取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL Google Cloud Storage]（以下「GCS」と呼ばれる）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを整理する際に使用する標準化されたExperience Platformフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターの UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な GCS 接続がある場合は、このドキュメントの残りの部分をスキップし、[ データフローの設定 ](../../dataflow/batch/cloud-storage.md) に関するチュートリアルに進んでください。

### サポートされているファイル形式

[!DNL Experience Platform] は、次のファイル形式をサポートし、外部ストレージから取り込むことができます。

* 区切り文字区切り値 (DSV):DSV 形式のデータ・ファイルのサポートは、現在、コンマ区切り値に制限されています。 DSV フォーマット・ファイル内のフィールド・ヘッダーの値は、英数字とアンダースコアのみで構成する必要があります。 一般的な DSV ファイルのサポートは、今後提供される予定です。
* JavaScript オブジェクト表記 (JSON):JSON 形式のデータファイルは、XDM に準拠している必要があります。
* Apache Parquet:Parquet 形式のデータファイルは、XDM に準拠している必要があります。

### 必要な資格情報の収集

[!DNL Platform] 上の GCS データにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| アクセスキー ID | [!DNL Google Cloud Storage] アカウントを Platform に対して認証するために使用される 61 文字の英数字の文字列。 |
| 秘密アクセスキー | [!DNL Google Cloud Storage] アカウントを Platform に対して認証するために使用される、40 文字のベース 64 エンコードされた文字列。 |

これらの値の詳細については、『Google Cloud Storage HMAC キー ](https://cloud.google.com/storage/docs/authentication/hmackeys#overview)』ガイドを参照してください。 [独自のアクセスキー ID と秘密アクセスキーを生成する手順については、[[!DNL Google Cloud Storage]  概要 ](../../../../connectors/cloud-storage/google-cloud-storage.md) を参照してください。

## [!DNL Google Cloud Storage] アカウントに接続

必要な資格情報を収集したら、以下の手順に従って GCS アカウントを [!DNL Platform] にリンクします。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、「**[!UICONTROL ソース]**」ワークスペースにアクセスします。 **[!UICONTROL カタログ]** 画面には、アカウントを作成するための様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的の特定のソースを見つけることもできます。

「**[!UICONTROL Databases]**」カテゴリで、「**[!UICONTROL Google Cloud Storage]**」を選択します。 このコネクタを初めて使用する場合は、「**[!UICONTROL 設定]**」を選択します。 それ以外の場合は、「**[!UICONTROL Add data]**」を選択して新しい GCS コネクタを作成します。

![カタログ](../../../../images/tutorials/create/google-cloud-storage/catalog.png)

「**[!UICONTROL Google Cloud Storage に接続]**」ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新規アカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、説明（オプション）および GCS 資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**] を選択し、新しい接続が確立されるまでしばらく時間をかけます。

![接続](../../../../images/tutorials/create/google-cloud-storage/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する GCS アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して次に進みます。

![既存](../../../../images/tutorials/create/google-cloud-storage/existing.png)

## 次の手順

このチュートリアルに従って、GCS アカウントへの接続を確立しました。 次のチュートリアルに進み、クラウドストレージから  [!DNL Platform]](../../dataflow/batch/cloud-storage.md) にデータを取り込むように [ データフローを設定します。
