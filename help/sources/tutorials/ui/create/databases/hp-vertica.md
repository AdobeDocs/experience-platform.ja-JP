---
keywords: Experience Platform；ホーム；人気のトピック；HP Vertica
solution: Experience Platform
title: UI での HP Vertica ソース接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して HP Vertica ソース接続を作成する方法を説明します。
exl-id: d7315ad4-9250-4e66-be33-016efabb512e
source-git-commit: e37c00863249e677f1645266859bf40fe6451827
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 39%

---

# HP の作成 [!DNL Vertica] UI のソース接続

>[!NOTE]
>
> HP [!DNL Vertica] コネクタはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータ版のコネクタの使用に関する詳細

Adobe Experience Platform のソースコネクタには、外部ソースの データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、HP を作成する手順を説明します [!DNL Vertica] を使用したソースコネクタ [!DNL Platform] ユーザーインターフェイス。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な HP がある場合 [!DNL Vertica] 接続する場合は、このドキュメントの残りの部分をスキップして、 [データフローの設定](../../dataflow/databases.md).

### 必要な資格情報の収集

次のセクションでは、HP に正しく接続するために知っておく必要がある追加情報を示します [!DNL Vertica] の使用 [!DNL Flow Service] API

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | HP への接続に使用する接続文字列 [!DNL Vertica] インスタンス。 HP の接続文字列パターン [!DNL Vertica] が `Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}` |

の導入について詳しくは、 [この HP [!DNL Vertica] 文書](https://www.vertica.com/docs/9.2.x/HTML/Content/Authoring/ConnectingToVertica/ClientJDBC/CreatingAndConfiguringAConnection.htm).

## HP を接続 [!DNL Vertica] アカウント

必要な資格情報を収集したら、次の手順に従って HP をリンクできます [!DNL Vertica] アカウント [!DNL Platform].

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL データベース]** カテゴリ、選択 **[!UICONTROL HP Vertica]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** 新しい HP を作成するには [!DNL Vertica] コネクタ。

![カタログ](../../../../images/tutorials/create/hp-vertica/catalog.png)

この **[!UICONTROL HP Vertica に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、HP を入力します [!DNL Vertica] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![接続](../../../../images/tutorials/create/hp-vertica/new.png)

### 既存のアカウント

既存のアカウントに接続するには、HP を選択します。 [!DNL Vertica] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして次に進みます。

![既存](../../../../images/tutorials/create/hp-vertica/existing.png)

## 次の手順

このチュートリアルに従って、HP との接続を確立しました。 [!DNL Vertica] アカウント 次のチュートリアルに進み、[データを に取り込むためのデータフローの設定 [!DNL Platform]](../../dataflow/databases.md)を行いましょう。
