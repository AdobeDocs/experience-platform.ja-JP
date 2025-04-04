---
title: UI [!DNL Google BigQuery]  使用したExperience Platformへの接続
description: Adobe Experience Platform UI を使用してGoogle Big Query ソース接続を作成する方法を説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 3c0902de-48b9-42d8-a4bd-0213ca85fc7f
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 14%

---

# UI を使用した [!DNL Google BigQuery] のExperience Platformへの接続

>[!IMPORTANT]
>
>Real-Time Customer Data Platform Ultimateを購入したユーザーは、ソースカタログで [!DNL Google BigQuery] ソースを利用できます。

ユーザーインターフェイスを使用して [!DNL Google BigQuery] アカウントをAdobe Experience Platformに接続する方法については、このチュートリアルをお読みください。

## 基本を学ぶ

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Google BigQuery] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

必要な認証情報の収集に関する詳細な手順については、[[!DNL Google BigQuery]  認証ガイド ](../../../../connectors/databases/bigquery.md#prerequisites) を参照してください。

## ソースカタログのナビゲート {#navigate}

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、*[!UICONTROL Sources]* ワークスペースにアクセスします。 *[!UICONTROL カテゴリ]* パネルで適切なカテゴリを選択できます。または、検索バーを使用して、使用する特定のソースに移動することもできます。

[!DNL Google BigQuery] を使用するには、「*[!UICONTROL データベース]*」の下の **[!UICONTROL Google BigQuery]** ソースカードを選択し、「**[!UICONTROL データを追加]**」を選択します。

>[!TIP]
>
>ソースカタログ内のソースは、特定のソースがまだ認証済みのアカウントを持っていない場合に「**[!UICONTROL 設定]**」オプションを表示します。 認証済みアカウントを作成すると、このオプションは **[!UICONTROL データを追加]** に変わります。

![Google BigQuery が選択されているソースカタログ ](../../../../images/tutorials/create/google-big-query/catalog.png)

## 既存のアカウントを使用 {#existing}

既存のアカウントを使用するには、接続する [!DNL Google BigQuery] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![ 既存のアカウントのリストが表示されている既存のアカウントページ。](../../../../images/tutorials/create/google-big-query/existing.png)

## 新しいアカウントを作成 {#create}

既存のアカウントがない場合は、ソースに対応する必要な認証資格情報を指定して、新しいアカウントを作成する必要があります。

新しいアカウントを作成するには、「**[!UICONTROL 新しいアカウント]**」を選択し、アカウントの名前を入力して、オプションで説明を追加します。

![ ソースワークフローの新しいアカウントインターフェイス ](../../../../images/tutorials/create/google-big-query/new.png)

### Azure 上のExperience Platformへの接続 {#azure}

基本認証またはサービス認証のいずれかを使用して、[!DNL Google BigQuery] アカウントを Azure 上のExperience Platformに接続できます。

>[!BEGINTABS]

>[!TAB  基本認証の使用 ]

基本認証を使用する場合は、「**[!UICONTROL 基本認証]**」を選択し、[ プロジェクト、クライアント ID、クライアント秘密鍵、更新トークン、（オプション）大きな結果データセット ID](../../../../connectors/databases/bigquery.md#generate-your-google-bigquery-credentials) の値を入力します。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、接続が確立されるまでしばらく待ちます。

![ 基本認証が選択された新しいアカウントインターフェイス ](../../../../images/tutorials/create/google-big-query/basic-auth.png)

>[!TAB  サービス認証の使用 ]

サービス認証を使用するには、「**[!UICONTROL サービス認証]**」を選択し、[ プロジェクト ID、キーファイルの内容、（オプション）大きな結果データセット ID](../../../../connectors/databases/bigquery.md#generate-your-google-bigquery-credentials) の値を指定します。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、接続が確立されるまでしばらく待ちます。

![ サービス認証が選択された新しいアカウントインターフェイス。](../../../../images/tutorials/create/google-big-query/service-auth.png)

>[!ENDTABS]

### Amazon Web ServicesのExperience Platform（AWS）への接続 {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../../../landing/multi-cloud.md) を参照してください。

新しい [!DNL Google BigQuery] アカウントを作成し、AWSでExperience Platformに接続するには、VA6 サンドボックスに属していることを確認し、認証に必要な資格情報を入力します。

* **プロジェクト ID**:[!DNL Google BigQuery] アカウントに対応するプロジェクト ID。
* **キーファイルの内容**: サービスアカウントの認証に使用されるキーファイル。 この値は [[!DNL Google Cloud service accounts] dashboard](https://console.cloud.google.com) から取得できます。 キーファイルのコンテンツは JSON 形式です。 Experience Platformへの認証時に、[!DNL Base64] でこれをエンコードする必要があります。
* **データセット ID**:[!DNL Google BigQuery] のデータセット ID。 この ID は、データテーブルがある場所を表し、大きな結果セットをサポートできるようにするには、事前に作成する必要があります。

![AWS接続用の新しいアカウントインターフェイス ](../../../../images/tutorials/create/google-big-query/aws.png)

## サンプルデータのプレビューをスキップ {#skip-preview-of-sample-data}

データ選択手順で、大きなテーブルまたはファイルのデータを取り込む際にタイムアウトが発生することがあります。 データプレビューをスキップして、タイムアウトを回避し、サンプルデータがなくてもスキーマを表示できます。 データのプレビューをスキップするには、「サンプルデータのプレビューをスキップ **[!UICONTROL 切替スイッチを有効]** します。

残りのワークフローは変わりません。 唯一の注意点は、データのプレビューをスキップすると、マッピングステップ中に計算フィールドと必須フィールドが自動検証されない可能性があり、マッピング中にこれらのフィールドを手動で検証する必要があるということです。

## 次の手順

このチュートリアルでは、[!DNL Google BigQuery] アカウントとの接続を確立しました。次のチュートリアルに進み、[ データをExperience Platformに取り込むためのデータフローの設定 ](../../dataflow/databases.md) を行いましょう。
