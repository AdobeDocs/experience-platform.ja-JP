---
keywords: Experience Platform；ホーム；人気のトピック；Teradata Vantage
title: UIでのTeradata Vantage Source Connectionの作成
description: Adobe Experience Platform UIを使用してTeradata Vantage ソース接続を作成する方法を説明します。
exl-id: 3fdb09fa-128a-477b-9144-d4ef3ed18ea6
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 20%

---

# UI での [!DNL Teradata Vantage] ソース接続の作成

このチュートリアルでは、Adobe Experience Platform ユーザーインターフェイスを使用して[!DNL Teradata Vantage] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [&#x200B; ソース &#x200B;](../../../../home.md): Experience Platformを使用すると、様々なソースからデータを取り込むことができますが、Experience Platform サービスを使用して着信データを構造化、ラベル付け、強化することができます。
* [&#x200B; サンドボックス &#x200B;](../../../../../sandboxes/home.md): Experience Platformは、1つのExperience Platform インスタンスを個別のバーチャル環境に分割して、デジタルエクスペリエンスアプリケーションの開発と進化に役立つバーチャルサンドボックスを提供します。

### 必要な資格情報の収集

Experience Platformで[!DNL Teradata Vantage] アカウントにアクセスするには、次の認証値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| 接続文字列 | 接続文字列とは、データソースとその接続方法に関する情報を提供する文字列です。 [!DNL Teradata Vantage]の接続文字列パターンは`DBCName={SERVER};Uid={USERNAME};Pwd={PASSWORD}`です。 |

開始の詳細については、この[[!DNL Teradata Vantage]  ドキュメント &#x200B;](https://docs.teradata.com/r/Teradata-VantageTM-Advanced-SQL-Engine-Security-Administration/July-2021/Setting-Up-the-Administrative-Infrastructure/Controlling-Access-to-the-Operating-System/Working-with-OS-Level-Security-Options)を参照してください。

## [!DNL Teradata Vantage] アカウントを接続

Experience Platform UIで、左側のナビゲーションから「**[!UICONTROL Sources]**」を選択して、[!UICONTROL Sources] ワークスペースにアクセスします。 画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

[!UICONTROL Databases] カテゴリで、**[!UICONTROL Teradata Vantage]**&#x200B;を選択し、**[!UICONTROL Set up]**&#x200B;を選択します。

>[!TIP]
>
>ソースカタログのソースには、特定のソースがまだ認証済みアカウントを持っていない場合、**[!UICONTROL Set up]** オプションが表示されます。 認証済みアカウントが存在すると、このオプションは&#x200B;**[!UICONTROL Add data]**&#x200B;に変更されます。

![Teradata Vantage ソースを選択したソースカタログ。](../../../../images/tutorials/create/teradata/catalog.png)

**[!UICONTROL Connect to Teradata Vantage]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを接続するには、接続する[!DNL Teradata Vantage] アカウントを選択し、**[!UICONTROL Next]**&#x200B;を選択して続行します。

![&#x200B; ソースワークスペースの既存のアカウントページ。](../../../../images/tutorials/create/teradata/existing.png)

### 新規アカウント

新しい資格情報を使用している場合は、**[!UICONTROL New account]**&#x200B;を選択します。 表示される入力フォームに、名前、オプションの説明、および[!DNL Teradata Vantage]資格情報を入力します。 完了したら、**[!UICONTROL Connect]**&#x200B;を選択し、新しい接続が確立されるまでしばらく時間を空けます。

![&#x200B; ソースワークスペースの新しいアカウント作成インターフェイス。](../../../../images/tutorials/create/teradata/new.png)

## 次の手順

このチュートリアルでは、Teradata Vantage アカウントへの接続を確立しました。 次のチュートリアルに進み、[&#x200B; データフローを設定してExperience Platformにデータを取り込むことができます](../../dataflow/databases.md)。
