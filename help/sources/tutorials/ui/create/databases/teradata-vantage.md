---
keywords: Experience Platform；ホーム；人気の高いトピック；TeradataVantage
title: UI でのTeradataVantage ソース接続の作成
description: Adobe Experience Platform UI を使用してTeradataVantage ソース接続を作成する方法を説明します。
source-git-commit: f140dac67ccd09ec1e6cab794f53e0090af55442
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 36%

---

# （ベータ版） [!DNL Teradata Vantage] UI のソース接続

>[!NOTE]
>
> この [!DNL Teradata Vantage] ソースはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

このチュートリアルでは、 [!DNL Teradata Vantage] Adobe Experience Platformユーザーインターフェイスを使用したソースコネクタ

## はじめに

このチュートリアルは、 Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Experience Platformサービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを個別の仮想環境に分割する仮想サンドボックスを提供し、デジタル体験アプリケーションの開発および進化を支援します。

### 必要な認証情報の収集

次の項目にアクセスするには、 [!DNL Teradata Vantage] プラットフォームのアカウントで、次の認証値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| 接続文字列 | 接続文字列は、データソースに関する情報とその接続方法を提供する文字列です。 次の接続文字列パターン： [!DNL Teradata Vantage] が `DBCName={SERVER};Uid={USERNAME};Pwd={PASSWORD}`. |

導入の詳細については、 [[!DNL Teradata Vantage] 文書](https://docs.teradata.com/r/Teradata-VantageTM-Advanced-SQL-Engine-Security-Administration/July-2021/Setting-Up-the-Administrative-Infrastructure/Controlling-Access-to-the-Operating-System/Working-with-OS-Level-Security-Options).

## [!DNL Teradata Vantage] アカウントを接続

Platform の UI で、左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索バーを使用して、利用したい特定のソースを見つけることもできます。

以下 [!UICONTROL データベース] カテゴリ、選択 **[!UICONTROL Teradata利益]** 次に、 **[!UICONTROL データを追加]**.

![](../../../../images/tutorials/create/teradata/catalog.png)

この **[!UICONTROL teradataVantage に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL Teradata Vantage] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![](../../../../images/tutorials/create/teradata/existing.png)

### 新しいアカウント

新しい資格情報を使用している場合は、「 **[!UICONTROL 新しいアカウント]**. 表示される入力フォームで、名前、説明（オプション）および [!DNL Teradata Vantage] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![](../../../../images/tutorials/create/teradata/new.png)

## 次の手順

このチュートリアルに従って、TeradataVantage アカウントへの接続を確立しました。 次のチュートリアルに進み、[データを Platform に取り込むためのデータフローの設定](../../dataflow/databases.md)を行いましょう。
