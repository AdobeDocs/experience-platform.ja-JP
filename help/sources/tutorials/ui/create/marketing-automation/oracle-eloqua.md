---
keywords: Experience Platform；ホーム；人気の高いトピック；ソース；コネクタ；oracle;oracleeloqua;eloqua
solution: Experience Platform
title: Platform UI を使用したOracleEloqua ソース接続の作成
topic-legacy: tutorial
description: Platform UI を使用してAdobe Experience PlatformをOracleEloqua に接続する方法を説明します。
exl-id: c4431d85-5948-4122-9a99-dbacdde5a09f
source-git-commit: 4c3988ea839c1d1843529eabcb0725f5b12feccc
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 40%

---

# の作成 [!DNL Oracle Eloqua] Platform UI を使用したソース接続

このチュートリアルでは、 [!DNL Oracle Eloqua] Adobe Experience Platformユーザーインターフェイスを使用したソースコネクタ

## はじめに

このガイドは、Adobe Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md):Platform を使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

既に [!DNL Oracle Eloqua] Platform のアカウントを使用する場合は、このドキュメントの残りの部分をスキップし、 [マーケティング自動化データを Platform に取り込むためのデータフローの作成](../../dataflow/marketing-automation.md).

### 必要な認証情報の収集

接続するには [!DNL Oracle Eloqua] を Platform に対して、次の認証プロパティの値を指定する必要があります。

| 認証情報 | 説明 |
| --- | --- |
| エンドポイント | のエンドポイント [!DNL Oracle Eloqua]. |
| ユーザー名 | ユーザー名 [!DNL Oracle Eloqua] アカウント ユーザー名は、 `siteName + \\ + username`で、 `siteName` は、ログインに使用した会社名です [!DNL Oracle Eloqua] および `username` はユーザー名です。 例えば、ログインユーザー名は次のようになります。 `adobe\\emily`. |
| パスワード | 次に対応するパスワード： [!DNL Oracle Eloqua] ユーザー名。 |

の認証資格情報の詳細 [!DNL Oracle Eloqua]を参照し、 [[!DNL Oracle Eloqua] 認証に関するガイド](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/Authentication_Basic.html).

必要な資格情報を収集したら、以下の手順に従って [!DNL Oracle Eloqua] アカウントを Platform にリンクできます。

## [!DNL Oracle Eloqua] アカウントを接続

Platform の UI で、左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 [!UICONTROL マーケティングの自動化] カテゴリ、選択 **[!UICONTROL OracleEloqua]**&#x200B;を選択し、 **[!UICONTROL データを追加]**.

![カタログ](../../../../images/tutorials/create/oracle-eloqua/catalog.png)

この **[!UICONTROL 接続OracleEloqua アカウント]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL Oracle Eloqua] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/oracle-eloqua/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「 **[!UICONTROL 新しいアカウント]**&#x200B;をクリックし、名前、説明（オプション）および [!DNL Oracle Eloqua] 資格情報。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![新規](../../../../images/tutorials/create/oracle-eloqua/new.png)

## 次の手順

このチュートリアルに従うことで、 [!DNL Oracle Eloqua] アカウントとプラットフォーム。 次のチュートリアルに進み、 [データフローを作成してマーケティング自動化データを Platform に取り込む](../../dataflow/marketing-automation.md).
