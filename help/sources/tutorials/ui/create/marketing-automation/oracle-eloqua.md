---
title: Platform UI を使用したOracleEloqua ソース接続の作成
description: Platform UI を使用してAdobe Experience PlatformをOracleEloqua に接続する方法を説明します。
exl-id: c4431d85-5948-4122-9a99-dbacdde5a09f
source-git-commit: e8f54f06ad3431227e140219a9960e8e04f83ccc
workflow-type: tm+mt
source-wordcount: '521'
ht-degree: 59%

---

# の作成 [!DNL Oracle Eloqua] Platform UI を使用したソース接続

このチュートリアルでは、 [!DNL Oracle Eloqua] Adobe Experience Platformユーザーインターフェイスを使用したソース接続

## はじめに

このガイドは、Adobe Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：Platform を使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび強化を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

認証済みの [!DNL Oracle Eloqua] アカウントが既に Platform にある場合は、このドキュメントの残りの部分をスキップし、[データフローを作成して Platform にマーケティング自動化データを取り込む](../../dataflow/marketing-automation.md)方法に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Oracle Eloqua] を Platform に接続するには、次の認証プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| エンドポイント | のエンドポイント [!DNL Oracle Eloqua] サーバー。 [!DNL Oracle Eloqua] は、複数のデータセンターをサポートしています。 エンドポイントを見つけるには、 [[!DNL Oracle Eloqua] インターフェイス](https://login.eloqua.com) 資格情報を使用して、リダイレクト URL からベース URL の部分をコピーします。 URL パターンの形式は次のとおりです。 `xxx.xx.eloqua.com` そして、 `http` または `https`. |
| ユーザー名 | ユーザー名 [!DNL Oracle Eloqua] サーバー。 ユーザー名は、 `siteName + \\ + username`で、 `siteName` は、ログインに使用した会社名です [!DNL Oracle Eloqua] および `username` はユーザー名です。 例えば、ログインユーザー名は次のようになります。 `Eloqua\Andy`. **注意**:バックスラッシュ (`\`)Experience PlatformUI は自動的にバックスラッシュ (`\`) をクリックします。 |
| パスワード | 次に対応するパスワード： [!DNL Oracle Eloqua] ユーザー名。 |

[!DNL Oracle Eloqua] の認証資格情報について詳しくは、[[!DNL Oracle Eloqua] 認証に関するガイド](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/Authentication_Basic.html)を参照してください。

必要な資格情報を収集したら、以下の手順に従って [!DNL Oracle Eloqua] アカウントを Platform にリンクできます。

## [!DNL Oracle Eloqua] アカウントを接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

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

このチュートリアルでは、認証を行い、お使いの [!DNL Oracle Eloqua] アカウントと Platform とのソース接続を作成しました。次のチュートリアルに進み、[マーケティング自動化データを Platform に取り込むためのデータフローを作成](../../dataflow/marketing-automation.md)できるようになりました。
