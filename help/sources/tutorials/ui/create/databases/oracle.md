---
title: UI を使用したOracle DB のExperience Platformへの接続
description: UI を使用してOracle DB インスタンスをExperience Platformに接続する方法について説明します。
exl-id: 4ca6ecc6-0382-4cee-acc5-1dec7eeb9443
source-git-commit: 7acdc090c020de31ee1a010d71a2969ec9e5bbe1
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 17%

---

# UI を使用した [!DNL Oracle DB] のExperience Platformへの接続

このガイドでは、Experience Platform ユーザーインターフェイスのソースワークスペースを使用して [!DNL Oracle DB] インスタンスをAdobe Experience Platformに接続する方法について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に [!DNL Oracle DB] 接続がある場合は、このドキュメントの残りの部分をスキップして、[&#x200B; データフローの設定 &#x200B;](../../dataflow/databases.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

認証について詳しくは、[[!DNL Oracle DB]  概要 &#x200B;](../../../../connectors/databases/oracle.md#prerequisites) を参照してください。

## ソースカタログのナビゲート

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、*[!UICONTROL Sources]* ワークスペースにアクセスします。 カテゴリを選択するか、検索バーを使用してソースを検索します。

[!DNL Oracle DB] に接続するには、[*[!UICONTROL データベース]*] カテゴリに移動し、**[!UICONTROL Oracle DB]** ソース カードを選択してから、[**[!UICONTROL 設定]**] を選択します。

>[!TIP]
>
>新しい接続の場合はソースに **[!UICONTROL 設定]** が表示され、アカウントが既に存在する場合は **[!UICONTROL データを追加]** が表示されます。

![&#x200B; 「Oracle DB」が選択されているソースカタログ &#x200B;](../../../../images/tutorials/create/oracle/catalog.png)

## 既存のアカウントを使用 {#existing}

既存のアカウントを使用するには、「**[!UICONTROL 既存のアカウント]**」を選択して、使用する [!DNL Oracle DB] アカウントを選択します。

![&#x200B; ソースワークフローの既存のアカウントインターフェイスで「既存のアカウント」が選択されている様子。](../../../../images/tutorials/create/oracle/existing.png)

## 新しいアカウントを作成 {#new}

新しいアカウントを作成するには、「**[!UICONTROL 新しいアカウント]**」を選択し、アカウントの名前を入力して、オプションで説明を追加します。

### Azure 上のExperience Platformへの接続 {#azure}

接続文字列を使用して、[!DNL Oracle DB] データベースを Azure 上のExperience Platformに接続できます。

接続文字列認証を使用するには、[&#x200B; 接続文字列 &#x200B;](../../../../connectors/databases/oracle.md#azure) を指定し、「**[!UICONTROL ソースに接続]**」を選択します。

![&#x200B; ソースワークフローで「接続文字列認証」が選択された新しいアカウントインターフェイス &#x200B;](../../../../images/tutorials/create/oracle/azure.png)

### Amazon Web ServicesのExperience Platform（AWS）への接続 {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../../../landing/multi-cloud.md) を参照してください。

新しい [!DNL Oracle DB] アカウントを作成し、AWSでExperience Platformに接続するには、VA6 サンドボックスに属していることを確認し、必要な [&#x200B; 認証用の資格情報 &#x200B;](../../../../connectors/databases/oracle.md#aws) を指定します。

![AWSに接続するための、ソースワークフローの新しいアカウントインターフェイス &#x200B;](../../../../images/tutorials/create/oracle/aws.png)

## データのデータフロー [!DNL Oracle DB] 作成

[!DNL Oracle DB] データベースに正常に接続できたので、[&#x200B; データフローを作成し、データベースからExperience Platformにデータを取り込む &#x200B;](../../dataflow/databases.md) ことができます。
