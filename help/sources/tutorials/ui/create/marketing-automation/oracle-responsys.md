---
keywords: Experience Platform;ホーム;人気のトピック;ソース;コネクタ;oracle;
title: （Beta）Experience Platform UI を使用したOracle Responsys ソース接続の作成
description: Experience Platform UI を使用してAdobe Experience PlatformをOracle Responsys に接続する方法を説明します。
hide: true
hidefromtoc: true
exl-id: 9ec5e1c2-3d9e-4729-be81-89a85d5ea782
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '491'
ht-degree: 54%

---

# （Beta）Experience Platform UI を使用した [!DNL Oracle Responsys] ソース接続の作成

>[!NOTE]
>
>[!DNL Oracle Responsys] ソースはベータ版です。ベータ版のコネクタの使用に関して詳しくは、[ ソースの概要 ](../../../../home.md#terms-and-conditions) を参照してください。

このチュートリアルでは、Adobe Experience Platform ユーザーインターフェイスを使用した [[!DNL Oracle Responsys]](../../../../connectors/marketing-automation/oracle-responsys.md) ソース接続を作成する手順を説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

認証済みの [!DNL Oracle Responsys] アカウントが既にExperience Platformにある場合は、このドキュメントの残りの部分をスキップし、[ データフローを作成してExperience Platformにマーケティング自動化データを取り込む ](../../dataflow/marketing-automation.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Oracle Responsys] をExperience Platformに接続するには、次の認証プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| エンドポイント | お使いの [!DNL Oracle Responsys] インスタンスの REST 認証エンドポイント URL。 |
| クライアント ID | お使いの [!DNL Oracle Responsys] インスタンスのクライアント ID。 |
| クライアントシークレット | お使いの [!DNL Oracle Responsys] インスタンスのクライアントシークレット。 |

[!DNL Oracle Responsys] の認証資格情報について詳しくは、[[!DNL Oracle Responsys] 認証に関するガイド](https://docs.oracle.com/en/cloud/saas/marketing/responsys-develop/API/GetStarted/authentication.htm)を参照してください。

必要な資格情報を収集したら、次の手順に従って [!DNL Oracle Responsys] アカウントをExperience Platformにリンクできます。

## [!DNL Oracle Responsys] アカウントを接続

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、[!UICONTROL Sources] ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

[!UICONTROL マーケティング自動化]のカテゴリで、**[!UICONTROL Oracle Responsys]** を選択したあと「**[!UICONTROL データを追加]**」を選択します。

![Oracle Responsys ソースがハイライト表示された Adobe Experience Platform ソースカタログ](../../../../images/tutorials/create/oracle-responsys/catalog.png)

**[!UICONTROL Oracle Responsys アカウントに接続]**&#x200B;ページが表示されます。このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL Oracle Responsys] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![Oracle Responsys の既存アカウント認証画面](../../../../images/tutorials/create/oracle-responsys/existing.png)

### 新規アカウント

新規アカウントを作成するには、「**[!UICONTROL 新規アカウント]**」を選択したあと、名前、説明（オプション）および [!DNL Oracle Responsys] 資格情報の適切な値を入力します。終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![Oracle Responsys の新規アカウント認証画面](../../../../images/tutorials/create/oracle-eloqua/new.png)

## 次の手順

このチュートリアルでは、認証を行い、お使いの [!DNL Oracle Responsys] アカウントとExperience Platformとのソース接続を作成しました。 次のチュートリアルに進み、[ マーケティング自動化データをExperience Platformに取り込むためのデータフローを作成する ](../../dataflow/marketing-automation.md) ことができるようになりました。
