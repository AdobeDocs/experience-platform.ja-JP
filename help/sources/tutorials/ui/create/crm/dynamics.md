---
keywords: Experience Platform；ホーム；人気のあるトピック；Microsoft Dynamics;microsoft dynamics;Dynamics;dynamics
solution: Experience Platform
title: UIでMicrosoft Dynamics Source Connectionを作成する
topic: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してMicrosoft Dynamicsソース接続を作成する方法を説明します。
translation-type: tm+mt
source-git-commit: c7fb0d50761fa53c1fdf4dd70a63c62f2dcf6c85
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 7%

---


# UIに[!DNL Microsoft Dynamics]ソース接続を作成する

このチュートリアルでは、Adobe Experience PlatformUIを使用して[!DNL Microsoft Dynamics]（以下「[!DNL Dynamics]」と呼ばれる）ソース接続を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効な[!DNL Dynamics]アカウントをお持ちの場合は、このドキュメントの残りの部分をスキップして、[CRMソースのデータフローの設定](../../dataflow/crm.md)のチュートリアルに進むことができます。

### 必要な資格情報の収集

| Credential | 説明 |
| ---------- | ----------- |
| `serviceUri` | [!DNL Dynamics]インスタンスのサービスURL。 |
| `username` | [!DNL Dynamics]ユーザーアカウントのユーザー名です。 |
| `password` | [!DNL Dynamics]アカウントのパスワード。 |
| `servicePrincipalId` | [!DNL Dynamics]アカウントのクライアントID。 このIDは、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |
| `servicePrincipalKey` | サービスプリンシパル秘密キー。 この秘密鍵証明書は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |

開始方法の詳細については、[この [!DNL Dynamics] ドキュメント](https://docs.microsoft.com/en-us/powerapps/developer/common-data-service/authenticate-oauth)を参照してください。

## [!DNL Dynamics]アカウントに接続

必要な資格情報を収集したら、次の手順に従って[!DNL Dynamics]アカウントをプラットフォームにリンクできます。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して[!UICONTROL ソース]ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

**[!UICONTROL CRM]**&#x200B;カテゴリの下で、**[!UICONTROL Microsoft Dynamics]**&#x200B;を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して新しい[!DNL Dynamics]コネクタを作成します。

![カタログ](../../../../images/tutorials/create/ms-dynamics/catalog.png)

**[!UICONTROL Dynamics]**&#x200B;に接続ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、新しい[!DNL Dynamics]アカウントの名前とオプションの説明を入力します。

[!DNL Dynamics]コネクタは、アクセス用に異なる認証タイプを提供します。 「[!UICONTROL アカウント認証]」で、「**[!UICONTROL 基本認証]**」を選択して、パスワードベースの秘密鍵証明書を使用します。

終了したら、[**[!UICONTROL ソースに接続]**]を選択し、新しいアカウントが確立されるまでの時間を許可します。

![基本認証](../../../../images/tutorials/create/ms-dynamics/basic-auth.png)

または、「**[!UICONTROL サービスプリンシパルとキー認証]**」を選択し、[!UICONTROL サービスプリンシパルID]と[!UICONTROL サービスプリンシパルキー]の組み合わせを使用して[!DNL Dynamics]アカウントを接続します。

>[!IMPORTANT]
>
> [!DNL Dynamics]での基本認証は、2要素認証でブロックされる可能性がありますが、プラットフォームでは現在サポートされていません。 この場合、[!DNL Dynamics]を使用してソースコネクタを作成するには、キーベースの認証を使用することをお勧めします。

![鍵ベース認証](../../../../images/tutorials/create/ms-dynamics/key-based-auth.png)

| Credential | 説明 |
| ---------- | ----------- |
| [!UICONTROL サービスプリンシパルID] | [!DNL Dynamics]アカウントのクライアントID。 このIDは、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |
| [!UICONTROL サービスプリンシパルキー] | サービスプリンシパル秘密キー。 この秘密鍵証明書は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |

### 既存のアカウント

既存のアカウントに接続するには、接続する[!DNL Dynamics]アカウントを選択し、右上隅の「**[!UICONTROL 次へ]**」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/ms-dynamics/existing.png)

## 次の手順

このチュートリアルに従うと、[!DNL Dynamics]アカウントへの接続が確立されます。 次のチュートリアルに進み、[データフローを設定して、データをプラットフォーム](../../dataflow/crm.md)に取り込むことができます。