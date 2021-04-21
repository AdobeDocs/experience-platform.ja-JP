---
keywords: Experience Platform；ホーム；人気のあるトピック；Salesforce;salesforce
solution: Experience Platform
title: UIでのSalesforceソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してSalesforceソース接続を作成する方法を説明します。
exl-id: b67fa4c4-d8ff-4d2d-aa76-5d9d32aa22d6
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 10%

---

# UIに[!DNL Salesforce]ソース接続を作成する

Adobe Experience Platformのソースコネクタは、外部ソースのCRMデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform]ユーザーインターフェイスを使用して[!DNL Salesforce]ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効な[!DNL Salesforce]アカウントをお持ちの場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/crm.md)のチュートリアルに進むことができます。

### 必要な資格情報の収集

| Credential | 説明 |
| ---------- | ----------- |
| `environmentUrl` | [!DNL Salesforce]ソースインスタンスのURL。 |
| `username` | [!DNL Salesforce]ユーザーアカウントのユーザー名です。 |
| `password` | [!DNL Salesforce]ユーザーアカウントのパスワードです。 |
| `securityToken` | [!DNL Salesforce]ユーザーアカウントのセキュリティトークン。 |

開始方法の詳細については、[このSalesforceドキュメント](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_authentication.htm)を参照してください。

## [!DNL Salesforce]アカウントに接続

必要な資格情報を収集したら、次の手順に従って[!DNL Salesforce]アカウントを[!DNL Platform]にリンクします。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して&#x200B;**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

**[!UICONTROL Databases]**&#x200B;カテゴリの下で、**[!UICONTROL Salesforce]**&#x200B;を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して、新しいSalesforceコネクタを作成します。

![カタログ](../../../../images/tutorials/create/salesforce/catalog.png)

**[!UICONTROL Salesforce]**&#x200B;に接続ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明、[!DNL Salesforce]資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/salesforce/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する[!DNL Salesforce]アカウントを選択し、右上隅の「**[!UICONTROL 次へ]**」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/salesforce/existing.png)

## 次の手順

このチュートリアルに従うと、[!DNL Salesforce]アカウントへの接続が確立されます。 次のチュートリアルに進み、[データを [!DNL Platform]](../../dataflow/crm.md)に取り込むようにデータフローを設定できます。
