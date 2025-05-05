---
title: UI を使用した MariaDB のExperience Platformへの接続
description: Experience Platform ユーザーインターフェイスのソースワークスペースを使用して、MariaDB アカウントをExperience Platformに接続する方法を説明します。
exl-id: 259ca112-01f1-414a-bf9f-d94caf4c69df
source-git-commit: 0bf31c76f86b4515688d3aa60deb8744e38b4cd5
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 16%

---

# UI を使用した [!DNL MariaDB] のExperience Platformへの接続

このガイドでは、Experience Platform ユーザーインターフェイスのソースワークスペースを使用して [!DNL MariaDB] アカウントをAdobe Experience Platformに接続する方法について説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集計したデータに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に [!DNL MariaDB] 接続がある場合は、このドキュメントの残りの部分をスキップして、[ データフローの設定 ](../../dataflow/databases.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

認証について詳しくは、[[!DNL MariaDB]  概要 ](../../../../connectors/databases/mariadb.md#prerequisites) を参照してください。

## ソースカタログのナビゲート

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、*[!UICONTROL Sources]* ワークスペースにアクセスします。 *[!UICONTROL カテゴリ]* パネルで適切なカテゴリを選択するか、検索バーを使用して、使用する特定のソースに移動します。

[!DNL MariaDB] を使用するには、「*[!UICONTROL データベース&#x200B;**[!UICONTROL 」の下の「]**&#x200B;MariaDB]*」ソースカードを選択し、「**[!UICONTROL 設定]**」を選択します。

>[!TIP]
>
>ソースカタログ内のソースは、特定のソースがまだ認証済みのアカウントを持っていない場合に「**[!UICONTROL 設定]**」オプションを表示します。 認証済みアカウントを作成すると、このオプションは **[!UICONTROL データを追加]** に変わります。

![MariaDB カードが選択された UI のソースカタログ ](../../../../images/tutorials/create/maria-db/catalog.png)

## 既存のアカウントを使用 {#existing}

既存のアカウントを使用するには、「**[!UICONTROL 既存のアカウント]**」を選択して、使用する [!DNL MariaDB] アカウントを選択します。

![ ソースワークフローの既存のアカウントインターフェイスで「既存のアカウント」が選択されている様子。](../../../../images/tutorials/create/maria-db/existing.png)

## 新しいアカウントを作成 {#create}

既存のアカウントがない場合は、ソースに対応する必要な認証資格情報を指定して、新しいアカウントを作成する必要があります。

新しいアカウントを作成するには、「**[!UICONTROL 新しいアカウント]**」を選択し、アカウントの名前を入力して、オプションで説明を追加します。

![ アカウント名とオプションの説明が表示された、ソースワークフローの新しいアカウントインターフェイス。](../../../../images/tutorials/create/maria-db/new.png)

### Azure 上のExperience Platformへの接続 {#azure}

アカウントキーまたは基本認証を使用して、[!DNL MariaDB] アカウントを Azure 上のExperience Platformに接続できます。

>[!BEGINTABS]

>[!TAB  アカウントキー認証 ]

アカウントキー認証を使用するには、「**[!UICONTROL アカウントキー認証]**」を選択し、「[ 接続文字列 ](../../../../connectors/databases/mariadb.md#azure)」を入力して、「**[!UICONTROL ソースに接続]**」を選択します。

![ 「アカウントキー認証」が選択されたソースワークフローの新しいアカウントインターフェイス ](../../../../images/tutorials/create/maria-db/account-key.png)

>[!TAB  基本認証 ]

基本認証を使用する場合は、「**[!UICONTROL 基本認証]**」を選択し、[ 認証資格情報 ](../../../../connectors/databases/mariadb.md#azure) の値を入力して「**[!UICONTROL ソースに接続]**」を選択します。

![ ソースワークフローで「基本認証」が選択された新しいアカウントインターフェイス ](../../../../images/tutorials/create/maria-db/basic-auth.png)

>[!ENDTABS]

### Amazon Web ServicesのExperience Platform（AWS）への接続 {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../../../landing/multi-cloud.md) を参照してください。

新しい [!DNL MariaDB] アカウントを作成し、AWSでExperience Platformに接続するには、VA6 サンドボックスに属していることを確認し、必要な [ 認証用の資格情報 ](../../../../connectors/databases/mariadb.md#aws) を指定します。

![AWSに接続するための、ソースワークフローの新しいアカウントインターフェイス ](../../../../images/tutorials/create/maria-db/basic-auth.png)

## 次の手順

このチュートリアルでは、[!DNL MariaDB] アカウントとの接続を確立しました。次のチュートリアルに進み、[ データをExperience Platformに取り込むためのデータフローの設定 ](../../dataflow/databases.md) を行いましょう。
