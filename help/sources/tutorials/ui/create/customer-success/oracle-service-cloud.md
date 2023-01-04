---
keywords: Experience Platform;ホーム;人気の高いトピック;Oracle Service Cloud;oracle service cloud
title: UI での Oracle Service Cloud ソース接続の作成
description: Adobe Experience Platform の UI を使用して Oracle サービスクラウドソース接続を作成する方法を説明します。
exl-id: e5869c09-b61e-4d23-a594-5a07769da3c4
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 100%

---

# （ベータ版）UI での Oracle Service Cloud ソース接続の作成

>[!NOTE]
>
>Oracle Service Cloud ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

このチュートリアルでは、Adobe Experience Platform ユーザーインターフェイスを使用して Oracle Service Cloud ソース接続を作成する手順について説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な Oracle Service Cloud 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/customer-success.md)に関するチュートリアルに進むことができます。

### 必要な認証情報の収集

[!DNL Platform] で Oracle Service Cloud アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| ホスト | Oracle Service Cloud インスタンスのホスト URL。 |
| ユーザー名 | Oracle Service Cloud ユーザーアカウントのユーザー名。 |
| パスワード | Oracle Service Cloud アカウントのパスワード。 |

Oracle Service Cloud アカウントの認証について詳しくは、[[!DNL Oracle] 認証に関するガイド](https://docs.oracle.com/en/cloud/saas/b2c-service/20c/cxska/OKCS_Authenticate_and_Authorize.html)を参照してください。

## Oracle Service Cloud アカウントへの接続

Platform の UI で、左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントの作成に使用できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索バーを使用して、利用したい特定のソースを見つけることもできます。

[!UICONTROL カスタマーサクセス]カテゴリで、「**[!UICONTROL Oracle Service Cloud]**」を選択し、次に「**[!UICONTROL データを追加]**」を選択します。

![ソースカタログと Oracle Service Cloud ソースがハイライト表示されています。](../../../../images/tutorials/create/oracle-service-cloud/catalog.png)

**[!UICONTROL Oracle Service Cloud への接続]**&#x200B;ページが表示されます。このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、接続する Oracle Service Cloud アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存のアカウントインターフェイス。](../../../../images/tutorials/create/oracle-service-cloud/existing.png)

### 新しいアカウント

新しい資格情報を使用している場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、アカウント名、説明（オプション）、Oracle Service Cloud の資格情報を入力します。終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![プレースホルダー値を含む新しいアカウントインターフェイス。](../../../../images/tutorials/create/oracle-service-cloud/new.png)

## 次の手順

このチュートリアルでは、Oracle Service Cloud アカウントとの接続を確立しました。次のチュートリアルに進み、[カスタマーサクセスデータを Platform に取り込むためのデータフローの設定](../../dataflow/crm.md)を行いましょう。
