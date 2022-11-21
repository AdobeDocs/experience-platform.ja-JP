---
keywords: Experience Platform；ホーム；人気の高いトピック；ソース；コネクタ；oracle;
title: （ベータ版）Platform UI を使用したOracleResponsys ソース接続の作成
description: Platform UI を使用してAdobe Experience PlatformをOracleResponsys に接続する方法を説明します。
hide: true
hidefromtoc: true
exl-id: 9ec5e1c2-3d9e-4729-be81-89a85d5ea782
source-git-commit: 784ec5f799c591185620e8376a6980b4930d914a
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 40%

---

# （ベータ版） [!DNL Oracle Responsys] Platform UI を使用したソース接続

>[!NOTE]
>
>この [!DNL Oracle Responsys] ソースはベータ版です。 ベータ版のコネクタの使用に関して詳しくは、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

このチュートリアルでは、 [[!DNL Oracle Responsys]](../../../../connectors/marketing-automation/oracle-responsys.md) Adobe Experience Platformユーザーインターフェイスを使用したソース接続

## はじめに

このガイドは、Adobe Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md):Platform を使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

既に [!DNL Oracle Responsys] Platform のアカウントを使用する場合は、このドキュメントの残りの部分をスキップし、 [マーケティング自動化データを Platform に取り込むためのデータフローの作成](../../dataflow/marketing-automation.md).

### 必要な認証情報の収集

接続するには [!DNL Oracle Responsys] を Platform に対して、次の認証プロパティの値を指定する必要があります。

| 認証情報 | 説明 |
| --- | --- |
| エンドポイント | の REST 認証エンドポイント URL [!DNL Oracle Responsys] インスタンス。 |
| クライアント ID | のクライアント ID [!DNL Oracle Responsys] インスタンス。 |
| クライアント秘密鍵 | お客様のクライアント秘密鍵 [!DNL Oracle Responsys] インスタンス。 |

の認証資格情報の詳細 [!DNL Oracle Responsys]を参照し、 [[!DNL Oracle Responsys] 認証に関するガイド](https://docs.oracle.com/en/cloud/saas/marketing/responsys-develop/API/GetStarted/authentication.htm).

必要な資格情報を収集したら、以下の手順に従って [!DNL Oracle Responsys] アカウントを Platform にリンクできます。

## [!DNL Oracle Responsys] アカウントを接続

Platform の UI で、左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 [!UICONTROL マーケティングの自動化] カテゴリ、選択 **[!UICONTROL OracleResponsys]**&#x200B;を選択し、 **[!UICONTROL データを追加]**.

![Adobe Experience PlatformソースカタログのOracleResponsys ソースが強調表示されています。](../../../../images/tutorials/create/oracle-responsys/catalog.png)

この **[!UICONTROL oracleResponsys アカウントの接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL Oracle Responsys] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![oracleResponsys の既存のアカウント認証画面。](../../../../images/tutorials/create/oracle-responsys/existing.png)

### 新しいアカウント

新しいアカウントを作成するには、 **[!UICONTROL 新しいアカウント]**&#x200B;をクリックし、名前、説明（オプション）および [!DNL Oracle Responsys] 資格情報。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![oracleResponsys の新しいアカウント認証画面。](../../../../images/tutorials/create/oracle-eloqua/new.png)

## 次の手順

このチュートリアルに従うことで、 [!DNL Oracle Responsys] アカウントとプラットフォーム。 次のチュートリアルに進み、 [データフローを作成してマーケティング自動化データを Platform に取り込む](../../dataflow/marketing-automation.md).
