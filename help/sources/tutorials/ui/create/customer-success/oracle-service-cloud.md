---
keywords: Experience Platform；ホーム；人気の高いトピック；Oracleサービスクラウド；oracleサービスクラウド
title: UI でのOracleサービスクラウドソース接続の作成
description: Adobe Experience Platform UI を使用してOracleサービスクラウドソース接続を作成する方法を説明します。
source-git-commit: 8e4f2170489d7e4e73bbc726e3253fac97c9f9f3
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 30%

---

# （ベータ版）UI でのOracleサービスクラウドソース接続の作成

>[!NOTE]
>
>oracleサービスクラウドソースはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

このチュートリアルでは、Adobe Experience Platformユーザーインターフェイスを使用してOracleサービスクラウドソース接続を作成する手順を説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):Experience Platformが顧客体験データを整理する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効なOracleサービスクラウドソース接続がある場合は、このドキュメントの残りの部分をスキップし、次のチュートリアルに進んでください。 [データフローの設定](../../dataflow/customer-success.md)

### 必要な認証情報の収集

次の場所でOracleサービスクラウドアカウントにアクセスする [!DNL Platform]に値を指定する場合は、次の値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| ホスト | oracleサービスクラウドインスタンスのホスト URL。 |
| ユーザー名 | Service Cloud ユーザーアカウントのOracle。 |
| パスワード | Service Cloud アカウントのOracle。 |

oracleサービスクラウドアカウントの認証について詳しくは、 [[!DNL Oracle] 認証に関するガイド](https://docs.oracle.com/en/cloud/saas/b2c-service/20c/cxska/OKCS_Authenticate_and_Authorize.html).

## oracleサービスクラウドアカウントに接続

Platform の UI で、左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。この [!UICONTROL カタログ] 画面には、アカウントの作成に使用できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索バーを使用して、利用したい特定のソースを見つけることもできます。

以下 [!UICONTROL 顧客の成功] カテゴリ、選択 **[!UICONTROL Oracleサービスクラウド]** 次に、 **[!UICONTROL データを追加]**.

![ソースカタログ内で、Oracleサービスクラウドソースが強調表示されています。](../../../../images/tutorials/create/oracle-service-cloud/catalog.png)

この **[!UICONTROL oracleサービスクラウドに接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントに接続するには、接続するOracleサービスクラウドアカウントを選択し、「 」を選択します **[!UICONTROL 次へ]** をクリックして続行します。

![既存のアカウントインターフェイス。](../../../../images/tutorials/create/oracle-service-cloud/existing.png)

### 新しいアカウント

新しい資格情報を使用している場合は、「 **[!UICONTROL 新しいアカウント]**. 表示される入力フォームで、名前、説明（オプション）およびOracleサービスクラウドの資格情報を入力します。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![のプレースホルダー値を含む新しいアカウントインターフェイス。](../../../../images/tutorials/create/oracle-service-cloud/new.png)

## 次の手順

このチュートリアルに従って、Oracleサービスクラウドアカウントへの接続を確立しました。 次のチュートリアルに進み、 [顧客の成功データを Platform に取り込むためのデータフローの設定](../../dataflow/crm.md).
