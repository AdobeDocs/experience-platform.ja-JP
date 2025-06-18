---
title: UI でのAzure Synapse Analytics Source接続の作成
description: Adobe Experience Platform UI を使用してAzure Synapse Analytics （以下「Synapse」）ソース接続を作成する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 1f1ce317-eaaf-4ad2-a5fb-236983220bd7
source-git-commit: f8eb8640360205e8ae9579d4b664d4880bf8a368
workflow-type: tm+mt
source-wordcount: '469'
ht-degree: 21%

---

# UI での [!DNL Azure Synapse Analytics] ソース接続の作成

>[!IMPORTANT]
>
>Real-Time Customer Data Platform Ultimateを購入したユーザーは、ソースカタログで [!DNL Azure Synapse Analytics] ソースを利用できます。

このガイドでは、UI のソースワークスペースを使用して [!DNL Azure Synapse Analytics] アカウントをAdobe Experience Platformに接続する方法について説明します。

## 基本を学ぶ

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Azure Synapse Analytics] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

認証について詳しくは、[[!DNL Azure Synapse Analytics]  概要 ](../../../../connectors/databases/synapse-analytics.md#prerequisites) を参照してください。

## ソースカタログのナビゲート

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、*[!UICONTROL Sources]* ワークスペースにアクセスします。 カテゴリを選択するか、検索バーを使用してソースを検索します。

[!DNL Azure Synapse Analytics] に接続するには、「*[!UICONTROL データベース]*」カテゴリに移動し、「**[!UICONTROL Azure Synapse analytics]**」ソースカードを選択して、「**[!UICONTROL 設定]**」を選択します。

>[!TIP]
>
>ソースカタログ内のソースは、特定のソースがまだ認証済みのアカウントを持っていない場合に「**[!UICONTROL 設定]**」オプションを表示します。 認証済みアカウントを作成すると、このオプションは **[!UICONTROL データを追加]** に変わります。

![ 「Azure Synapse Analytics」が選択されているソースカタログ ](../../../../images/tutorials/create/azure-synapse-analytics/catalog.png)

## 既存のアカウントを使用 {#existing}

既存のアカウントを使用するには、「**[!UICONTROL 既存のアカウント]**」を選択して、使用する [!DNL Azure Synapse Analytics] アカウントを選択します。

![ ソースワークフローの既存のアカウントインターフェイス。](../../../../images/tutorials/create/azure-synapse-analytics/existing.png)

## 新しいアカウントを作成 {#new}

新しいアカウントを作成するには、「**[!UICONTROL 新しいアカウント]**」を選択し、アカウントの名前を入力して、オプションで説明を追加します。

![ ソースワークフローの新しいアカウントインターフェイス ](../../../../images/tutorials/create/azure-synapse-analytics/new.png)

### Experience Platformへの接続

アカウントキー認証またはサービスプリンシパルおよびキー認証のいずれかを使用して、[!DNL Azure Synapse Analytics] アカウントをExperience Platformに接続できます。

>[!BEGINTABS]

>[!TAB  アカウントキー認証 ]

アカウントキー認証を使用するには、「**[!UICONTROL アカウントキー認証]**」を選択し、「[ 接続文字列 ](../../../../connectors/databases/synapse-analytics.md#prerequisites)」を入力して、「**[!UICONTROL ソースに接続]**」を選択します。

![ ソースワークフローの「新しいアカウントを作成」手順で「アカウントキー認証 ](../../../../images/tutorials/create/azure-synapse-analytics/account-key-auth.png) が選択されている。

>[!TAB  サービスプリンシパルとキーの認証 ]

または、「**[!UICONTROL サービスプリンシパルとキー認証]**」を選択し、[ 認証資格情報 ](../../../../connectors/databases/synapse-analytics.md#prerequisites) の値を入力して、「**[!UICONTROL ソースに接続]**」を選択します。

![ ソースワークフローの「新しいアカウントを作成」手順で「サービスプリンシパルとキー認証」が選択されている様子。](../../../../images/tutorials/create/azure-synapse-analytics/service-principal.png)

>[!ENDTABS]

## データのデータフロー [!DNL Azure Synapse Analytics] 作成

[!DNL Azure Synapse Analytics] データベースに正常に接続できたので、[ データフローを作成し、データベースからExperience Platformにデータを取り込む ](../../dataflow/databases.md) ことができます。
