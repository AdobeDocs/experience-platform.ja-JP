---
title: '[Experience Platform] ユーザーインターフェイスを使用して Phoenix アカウントに接続する'
description: ユーザーインターフェイスを使用して Phoenix アカウントを接続し、Phoenix データベースからExperience Platformにデータを取り込む方法を説明します。
exl-id: 2ed469bc-1c72-4f04-a5f0-6a0bb519a6c2
source-git-commit: b7e42eb180b8f16344afedadf763c33bcf22fa35
workflow-type: tm+mt
source-wordcount: '604'
ht-degree: 25%

---

# 接続する [!DNL Phoenix] UI を使用してExperience Platformにアカウント

このチュートリアルでは、 [!DNL Phoenix] アカウントとデータの取り込み [!DNL Phoenix] データベースからExperience Platformへ。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に [!DNL Phoenix] アカウントを使用する場合は、このドキュメントの残りの部分をスキップし、次のチュートリアルに進んでください。 [データベースのデータフローの設定](../../dataflow/databases.md).

### 必要な資格情報の収集

次の項目にアクセスするには、 [!DNL Phoenix] Experience Platformのアカウントでは、次の値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| ホスト | の IP アドレスまたはホスト名 [!DNL Phoenix] サーバー。 |
| ポート | TCP ポート [!DNL Phoenix] サーバーは、を使用してクライアント接続をリッスンします。 次に接続する場合： [!DNL Azure HDInsights]で、ポートを 443 に指定します。 このパラメーターを指定しない場合、値のデフォルトは 8765 です。 |
| HTTP パス | URL の一部 [!DNL Phoenix] サーバー。 /hbasephoenix0 を指定します。 [!DNL Azure HDInsights] クラスター。 |
| ユーザー名 | 次にアクセスするために使用するユーザー名： [!DNL Phoenix] サーバー。 |
| パスワード | ユーザーに対応するパスワード。 |
| Enable SSL | SSL を使用してサーバーへの接続を暗号化するかどうかを指定するトグル。 |

の導入について詳しくは、 [この [!DNL Phoenix] 文書](https://python-phoenixdb.readthedocs.io/en/latest/api.html).

必要な資格情報を収集したら、次の手順に従って、 [!DNL Phoenix] アカウントからExperience Platformへ。

## [!DNL Phoenix] アカウントを接続

Platform UI で、「 」を選択します。 **[!UICONTROL ソース]** 左側のナビゲーションからソースワークスペースにアクセスします。 The *[!UICONTROL カタログ]* 画面には、「ソース」カタログで使用可能な様々なソースがExperience Platformされます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索オプションを使用して特定のソースを検索できます。

選択 **[!UICONTROL データベース]** ソースカテゴリのリストから、「 」を選択します。 **[!UICONTROL データを追加]** から [!DNL Phoenix] カード。

>[!TIP]
>
>ソースカタログ内のソースには、ソースのステータスに応じて異なるプロンプトが表示される場合があります。
> 
>* **[!UICONTROL データを追加]** は、選択したソースに関連付けられている既存の認証済みアカウントがあることを意味します。
>
>* **[!UICONTROL 設定]** は、選択したソースを使用するには、資格情報を入力し、新しいアカウントを認証する必要があることを意味します。

![Phoenix ソースカードが選択されたExperience PlatformUI 上のソースカタログ。](../../../../images/tutorials/create/phoenix/catalog.png)

The **[!UICONTROL Phoenix に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

>[!BEGINTABS]

>[!TAB 既存の Phoenix アカウントを使用]

既存のアカウントを使用するには、「[!UICONTROL 既存のアカウント]」を選択し、表示されるリストから使用するアカウントを選択します。終了したら、「 」を選択します。 [!UICONTROL 次へ] をクリックして続行します。

![組織に既に存在する認証済みの Phoenix データベースアカウントのリスト。](../../../../images/tutorials/create/phoenix/existing.png)

>[!TAB 新しい Phoenix アカウントを作成]

新しいアカウントを使用するには、 [!UICONTROL 新しいアカウント] 名前、説明、 [!DNL Phoenix] 認証資格情報。 終了したら、「 」を選択します。 [!UICONTROL ソースに接続] そして、新しい接続が確立されるまで数秒間待ちます。

![認証資格情報を入力し、Phoenix アカウントを作成できる新しいアカウントインターフェイス。](../../../../images/tutorials/create/phoenix/new.png)

>[!ENDTABS]

## 次の手順

このチュートリアルでは、[!DNL Phoenix] アカウントとの接続を確立しました。次のチュートリアルに進み、 [データをExperience Platformに取り込むようにデータフローを設定](../../dataflow/databases.md).
