---
title: UI を使用したAWS Redshift のExperience Platformへの接続
description: ソース UI を使用してAWS Redshift アカウントをExperience Platformに接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 4faf3200-673b-4a20-8f94-d049e800444b
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '719'
ht-degree: 20%

---

# UI を使用した [!DNL AWS Redshift] のExperience Platformへの接続

>[!IMPORTANT]
>
>Real-Time Customer Data Platform Ultimateを購入したユーザーは、ソースカタログで [!DNL AWS Redshift] ソースを利用できます。

このガイドでは、UI のソースワークスペースを使用して [!DNL AWS Redshift] アカウントをAdobe Experience Platformに接続する方法について説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL AWS Redshift] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)に関するチュートリアルに進むことができます。

## ソースカタログのナビゲート

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、[!UICONTROL Sources] ワークスペースにアクセスします。 画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

*[!UICONTROL データベース]* カテゴリの下の「**[!DNL AWS Redshift]**」を選択し、「**[!UICONTROL 設定]**」を選択します。

>[!TIP]
>
>ソースカタログ内のソースは、特定のソースがまだ認証済みのアカウントを持っていない場合に「**[!UICONTROL 設定]**」オプションを表示します。 認証済みアカウントが存在すると、このオプションは **[!UICONTROL データを追加]** に変わります。

![AWS Redshift ソースカードが選択されているソースカタログ &#x200B;](../../../../images/tutorials/create/redshift/catalog.png)

## 既存のアカウントを使用 {#existing}

次に、ソースワークフローの認証手順に進みます。 ここでは、既存のアカウントを使用するか、新しいアカウントを作成できます。

既存のアカウントを使用するには、アカウントディレクトリから [!DNL AWS Redshift] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![&#x200B; ソースワークフローのアカウントディレクトリが表示されます。ここでは、既存のアカウントが表示されます。](../../../../images/tutorials/create/redshift/existing.png)

## 新しいアカウントを作成 {#create}

既存のアカウントがない場合は、ソースに対応する必要な認証資格情報を指定して、新しいアカウントを作成する必要があります。

新しいアカウントを作成するには、「**[!UICONTROL 新しいアカウント]**」を選択し、アカウントの名前を入力して、オプションで説明を追加します。

### Azure 上のExperience Platformへの接続 {#azure}

[!DNL AWS Redshift] アカウントを Azure 上のExperience Platformに接続するには、入力フォームに認証資格情報を入力し、「**（[!UICONTROL &#x200B; ソースに接続 &#x200B;]）**」を選択します。

![AWS Redshift を Azure 上のExperience Platformに接続するための新しいアカウントインターフェイス &#x200B;](../../../../images/tutorials/create/redshift/new.png)

| 資格情報 | 説明 |
| --- | --- |
| サーバー | [!DNL AWS Redshift] インスタンスのサーバー名。 |
| ポート | クライアント接続をリッスンするために [!DNL AWS Redshift] サーバーが使用する TCP ポート。 |
| ユーザー名 | アクセス権を付与するアカウントのユーザー名。 |
| パスワード | ユーザーアカウントに対応するパスワード。 |
| データベース | データの取得元となる [!DNL AWS Redshift] データベース。 |

基本について詳しくは、[&#x200B; このドキュメント  [!DNL AWS Redshift]  を参照してください &#x200B;](https://docs.aws.amazon.com/redshift/latest/gsg/new-user-serverless.html)。

### AWS上のExperience Platformへの接続 {#aws}

>[!AVAILABILITY]
>
>この節の内容は、AWS web サービス（AWS）上で動作するExperience Platformの実装に当てはまります。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../../../landing/multi-cloud.md) を参照してください。

新しい [!DNL AWS Redshift] アカウントを作成し、AWSでExperience Platformに接続するには、VA6 サンドボックスに属していることを確認し、認証に必要な資格情報を入力して、「**[!UICONTROL ソースに接続]**」を選択します。

![AWS Redshift をAWS上のExperience Platformに接続するための新しいアカウントインターフェイス &#x200B;](../../../../images/tutorials/create/redshift/aws-auth.png)

| 資格情報 | 説明 |
| --- | --- |
| サーバー | [!DNL AWS Redshift] インスタンスのサーバー名。 |
| ポート | クライアント接続をリッスンするために [!DNL AWS Redshift] サーバーが使用する TCP ポート。 |
| ユーザー名 | アクセス権を付与するアカウントのユーザー名。 |
| パスワード | ユーザーアカウントに対応するパスワード。 |
| データベース | データの取得元となる [!DNL AWS Redshift] データベース。 |
| スキーマ | [!DNL AWS Redshift] データベースに関連付けられたスキーマの名前。 データベースアクセス権を付与するユーザーが、このスキーマにもアクセスできることを確認する必要があります。 |

基本について詳しくは、[&#x200B; このドキュメント  [!DNL AWS Redshift]  を参照してください &#x200B;](https://docs.aws.amazon.com/redshift/latest/gsg/new-user-serverless.html)。

## 次の手順

このチュートリアルでは、[!DNL AWS Redshift] データベースとExperience Platformの間の接続を確立しました。 次のチュートリアルに進み、[&#x200B; データフローを作成して、データベースからExperience Platformにデータを取り込む &#x200B;](../../dataflow/databases.md) ことができます。