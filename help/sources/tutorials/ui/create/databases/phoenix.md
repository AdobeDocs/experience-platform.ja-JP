---
title: Experience Platformユーザーインターフェイスを使用して Phoenix アカウントを接続します
description: ユーザーインターフェイスを使用して Phoenix アカウントを接続し、Phoenix データベースからExperience Platformにデータを取り込む方法を説明します。
exl-id: 2ed469bc-1c72-4f04-a5f0-6a0bb519a6c2
source-git-commit: b7e42eb180b8f16344afedadf763c33bcf22fa35
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 22%

---

# UI を使用して [!DNL Phoenix] アカウントをExperience Platformに接続する

このチュートリアルでは、[!DNL Phoenix] アカウントを接続し、[!DNL Phoenix] データベースからExperience Platformにデータを取り込む手順について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

認証済みの [!DNL Phoenix] アカウントが既にある場合は、このドキュメントの残りの部分をスキップし、[ データベースのデータフローの設定 ](../../dataflow/databases.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

Experience Platformで [!DNL Phoenix] アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| ホスト | [!DNL Phoenix] サーバーの IP アドレスまたはホスト名。 |
| ポート | クライアント接続をリッスンするために [!DNL Phoenix] サーバーが使用する TCP ポート。 [!DNL Azure HDInsights] に接続する場合は、ポートを 443 に指定します。 このパラメーターを指定しない場合、値はデフォルトで 8765 になります。 |
| HTTP パス | [!DNL Phoenix] サーバーに対応する部分的な URL。 [!DNL Azure HDInsights] クラスタを使用している場合は、/hbasephoenix0 を指定します。 |
| ユーザー名 | [!DNL Phoenix] サーバーへのアクセスに使用するユーザー名。 |
| パスワード | ユーザーに対応するパスワード。 |
| Enable SSL | サーバーへの接続を SSL を使用して暗号化するかどうかを指定するトグルです。 |

基本について詳しくは、[ このドキュメント  [!DNL Phoenix]  を参照してください ](https://python-phoenixdb.readthedocs.io/en/latest/api.html)。

必要な資格情報を収集したら、次の手順に従って [!DNL Phoenix] アカウントをExperience Platformに接続できます。

## [!DNL Phoenix] アカウントを接続

Platform UI の左側のナビゲーションで「**[!UICONTROL ソース]**」を選択し、ソース ワークスペースにアクセスします。 *[!UICONTROL カタログ]* 画面には、Experience Platformソースカタログで使用可能な様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索オプションを使用して特定のソースを見つけることもできます。

ソースカテゴリのリストから **[!UICONTROL データベース]** を選択し、[!DNL Phoenix] ールカードから **[!UICONTROL データを追加]** を選択します。

>[!TIP]
>
>ソースカタログ内のソースでは、ソースのステータスに応じて異なるプロンプトが表示される場合があります。
> 
>* **[!UICONTROL データを追加]** とは、選択したソースに関連付けられた既存の認証済みアカウントがあることを意味します。
>
>* **[!UICONTROL 設定]** とは、選択したソースを使用するために、資格情報を提供し、新しいアカウントを認証する必要があることを意味します。

![Phoenix ソースカードが選択されているExperience PlatformUI のソースカタログ ](../../../../images/tutorials/create/phoenix/catalog.png)

**[!UICONTROL Phoenix に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

>[!BEGINTABS]

>[!TAB  既存の Phoenix アカウントを使用する ]

既存のアカウントを使用するには、「[!UICONTROL  既存のアカウント ]」を選択し、表示されるリストから使用するアカウントを選択します。 終了したら、「[!UICONTROL  次へ ] を選択して続行します。

![ 組織に既に存在する、認証済みの Phoenix データベースアカウントのリスト。](../../../../images/tutorials/create/phoenix/existing.png)

>[!TAB  新しい Phoenix アカウントの作成 ]

新しいアカウントを使用するには、「[!UICONTROL  新しいアカウント ]」を選択し、名前、説明および [!DNL Phoenix] 認証資格情報を入力します。 終了したら「[!UICONTROL  ソースに接続 ]」を選択し、新しい接続が確立されるまで数秒間待ちます。

![ 認証資格情報を指定し、Phoenix アカウントを作成できる新しいアカウントインターフェイス。](../../../../images/tutorials/create/phoenix/new.png)

>[!ENDTABS]

## 次の手順

このチュートリアルでは、[!DNL Phoenix] アカウントとの接続を確立しました。次のチュートリアルに進み、[ データをExperience Platformに取り込むためのデータフローの設定 ](../../dataflow/databases.md) を行いましょう。
