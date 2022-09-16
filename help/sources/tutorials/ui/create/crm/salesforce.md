---
keywords: Experience Platform；ホーム；人気の高いトピック；Salesforce;Salesforce
solution: Experience Platform
title: UI での Salesforce ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して Salesforce ソース接続を作成する方法を説明します。
exl-id: b67fa4c4-d8ff-4d2d-aa76-5d9d32aa22d6
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 58%

---

# UI での [!DNL Salesforce] ソース接続の作成

Adobe Experience Platform のソースコネクタには、外部ソースの CRM データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Platform] のユーザーインターフェイスを使用して [!DNL Salesforce] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

有効な [!DNL Salesforce] アカウントを既にお持ちの場合は、このドキュメントの残りの部分をスキップし、[データフローの設定](../../dataflow/crm.md)に関するチュートリアルに進んでください。

### 必要な認証情報の収集

| 認証情報 | 説明 |
| ---------- | ----------- |
| `environmentUrl` | の URL [!DNL Salesforce] ソースインスタンス。 |
| `username` | のユーザー名 [!DNL Salesforce] ユーザーアカウント。 |
| `password` | のパスワード [!DNL Salesforce] ユーザーアカウント。 |
| `securityToken` | のセキュリティトークン [!DNL Salesforce] ユーザーアカウント。 |

の導入について詳しくは、 [この Salesforce ドキュメント](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_authentication.htm).

## [!DNL Salesforce] アカウントの接続

必要な認証情報が揃ったら、次の手順に従って、[!DNL Salesforce] アカウントを [!DNL Platform] にリンクします。

にログインします。 [Adobe Experience Platform](https://platform.adobe.com) 次に、 **[!UICONTROL ソース]** 左側のナビゲーションバーから **[!UICONTROL ソース]** ワークスペース。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL データベース]** カテゴリ、選択 **[!UICONTROL Salesforce]**. このコネクタを初めて使用する場合は、「 **[!UICONTROL 設定]**. それ以外の場合は、「 **[!UICONTROL データを追加]** 新しい Salesforce コネクタを作成します。

![カタログ](../../../../images/tutorials/create/salesforce/catalog.png)

この **[!UICONTROL Salesforce に連携]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 新しいアカウント

新しい資格情報を使用している場合は、「 **[!UICONTROL 新しいアカウント]**. 表示される入力フォームで、名前、説明（オプション）および [!DNL Salesforce] 資格情報。 終了したら、「 」を選択します。 **[!UICONTROL 接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![接続](../../../../images/tutorials/create/salesforce/new.png)

### 既存のアカウント

既存のアカウントに接続するには、 [!DNL Salesforce] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして次に進みます。

![既存](../../../../images/tutorials/create/salesforce/existing.png)

## 次の手順

このチュートリアルでは、[!DNL Salesforce] アカウントとの接続を確立しました。次のチュートリアルに進み、[データを に取り込むためのデータフローの設定 [!DNL Platform]](../../dataflow/crm.md)を行いましょう。
