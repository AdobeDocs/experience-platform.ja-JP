---
keywords: Experience Platform；ホーム；人気のトピック；Phoenix；フェニックス
solution: Experience Platform
title: UI での Phoenix ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して Phoenix ソース接続を作成する方法を説明します。
exl-id: 2ed469bc-1c72-4f04-a5f0-6a0bb519a6c2
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 48%

---

# UI での [!DNL Phoenix] ソース接続の作成

>[!NOTE]
>
> この [!DNL Phoenix] コネクタはベータ版です。 ベータ版のコネクタの使用に関して詳しくは、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Platform] のユーザーインターフェイスを使用して [!DNL Phoenix] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Phoenix] 接続する場合は、このドキュメントの残りの部分をスキップして、 [データフローの設定](../../dataflow/databases.md)

### 必要な資格情報の収集

 で [!DNL Phoenix] アカウントにアクセスするには、次の値を指定する必要があります。[!DNL Platform]

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | の IP アドレスまたはホスト名 [!DNL Phoenix] サーバー。 |
| `port` | TCP ポート [!DNL Phoenix] サーバーは、を使用してクライアント接続をリッスンします。 次に接続する場合： [!DNL Azure HDInsights]で、ポートを 443 と指定します。 |
| `httpPath` | URL の一部 [!DNL Phoenix] サーバー。 /hbasephoenix0 を指定します。 [!DNL Azure HDInsights] クラスター。 |
| `username` | 次にアクセスするために使用するユーザー名 [!DNL Phoenix] サーバー。 |
| `password` | ユーザーに対応するパスワード。 |
| `enableSsl` | SSL を使用してサーバーへの接続を暗号化するかどうかを指定するトグル。 |

の導入について詳しくは、 [この [!DNL Phoenix] 文書](https://python-phoenixdb.readthedocs.io/en/latest/api.html).

## [!DNL Phoenix] アカウントを接続

必要な資格情報を収集したら、次の手順に従って、 [!DNL Phoenix] 接続するアカウント [!DNL Platform].

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL データベース]** カテゴリ、選択 **[!UICONTROL Phoenix]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** 新しい [!DNL Phoenix] アカウント

![カタログ](../../../../images/tutorials/create/phoenix/catalog.png)

この **[!UICONTROL Phoenix に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）および [!DNL Phoenix] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![接続](../../../../images/tutorials/create/phoenix/new.png)

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL Phoenix] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/phoenix/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Phoenix] アカウントとの接続を確立しました。次のチュートリアルに進み、[データを に取り込むためのデータフローの設定 [!DNL Platform]](../../dataflow/databases.md)を行いましょう。
