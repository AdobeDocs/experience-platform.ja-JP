---
keywords: Experience Platform；ホーム；人気のトピック；Teradata Vantage
title: UI でのTeradata Vantage Source接続の作成
description: Adobe Experience Platform UI を使用してTeradata Vantage ソース接続を作成する方法を説明します。
exl-id: 3fdb09fa-128a-477b-9144-d4ef3ed18ea6
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '418'
ht-degree: 21%

---

# UI での [!DNL Teradata Vantage] ソース接続の作成

このチュートリアルでは、Adobe Experience Platform ユーザーインターフェイスを使用して [!DNL Teradata Vantage] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### 必要な資格情報の収集

Experience Platformで [!DNL Teradata Vantage] アカウントにアクセスするには、次の認証値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| 接続文字列 | 接続文字列は、データ ソースとその接続方法に関する情報を提供する文字列です。 [!DNL Teradata Vantage] の接続文字列のパターンは `DBCName={SERVER};Uid={USERNAME};Pwd={PASSWORD}` です。 |

基本について詳しくは、この [[!DNL Teradata Vantage]  ドキュメント ](https://docs.teradata.com/r/Teradata-VantageTM-Advanced-SQL-Engine-Security-Administration/July-2021/Setting-Up-the-Administrative-Infrastructure/Controlling-Access-to-the-Operating-System/Working-with-OS-Level-Security-Options) を参照してください。

## [!DNL Teradata Vantage] アカウントを接続

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、[!UICONTROL Sources] ワークスペースにアクセスします。 画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

[!UICONTROL Databases] カテゴリで、[**[!UICONTROL Teradata Vantage]**] を選択し、[**[!UICONTROL 設定]**] を選択します。

>[!TIP]
>
>ソースカタログ内のソースは、特定のソースがまだ認証済みのアカウントを持っていない場合に「**[!UICONTROL 設定]**」オプションを表示します。 認証済みアカウントが存在すると、このオプションは **[!UICONTROL データを追加]** に変わります。

![ ソースカタログとTeradata Vantage ソースが選択されています。](../../../../images/tutorials/create/teradata/catalog.png)

**[!UICONTROL Teradata Vantage に接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Teradata Vantage] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![ ソースワークスペースの既存のアカウントページ。](../../../../images/tutorials/create/teradata/existing.png)

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、名前、説明（オプション）、[!DNL Teradata Vantage] 資格情報を入力します。 終了したら「**[!UICONTROL 接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![ ソースワークスペースの新しいアカウント作成インターフェイス ](../../../../images/tutorials/create/teradata/new.png)

## 次の手順

このチュートリアルでは、Teradata Vantage アカウントとの接続を確立しました。 次のチュートリアルに進み、[ データをExperience Platformに取り込むためのデータフローの設定 ](../../dataflow/databases.md) を行いましょう。
