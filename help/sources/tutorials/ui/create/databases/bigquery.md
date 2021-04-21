---
keywords: Experience Platform；ホーム；人気のあるトピック；Google Bigクエリ;Googleビッグクエリ;GBQ;gbq
solution: Experience Platform
title: UIでのGoogle Bigクエリソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してGoogle Bigクエリソース接続を作成する方法を説明します。
exl-id: 3c0902de-48b9-42d8-a4bd-0213ca85fc7f
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 8%

---

# UIに[!DNL Google Big Query]ソース接続を作成する

>[!NOTE]
>
> [!DNL Google BigQuery]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform]ユーザーインターフェイスを使用して[!DNL Google Big Query] （以下「BigQuery」と呼びます）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効なBigQueryドキュメントをお持ちの場合は、この接続の残りの部分をスキップし、[データフロー](../../dataflow/databases.md)の設定に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Platform]のBigQueryアカウントにアクセスするには、次のOAuth 2.0認証値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `project` | クエリするデフォルトの[!DNL BigQuery]プロジェクトのプロジェクトID。 |
| `clientID` | 更新トークンの生成に使用されるID値。 |
| `clientSecret` | 更新トークンの生成に使用されるシークレット値。 |
| `refreshToken` | [!DNL BigQuery]へのアクセスを許可するために使用される[!DNL Google]から取得される更新トークン。 |

これらの値の詳細については、[このBigQueryドキュメント](https://cloud.google.com/storage/docs/json_api/v1/how-tos/authorizing)を参照してください。

## Google BigQueryアカウントに接続する

必要な資格情報を収集したら、次の手順に従ってBigQueryアカウントを[!DNL Platform]にリンクします。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して&#x200B;**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

**[!UICONTROL Databases]**&#x200B;カテゴリーの下で、**[!UICONTROL Google Bigクエリ]**&#x200B;を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して、新しいBigQueryコネクタを作成します。

![](../../../../images/tutorials/create/google-big-query/catalog.png)

「**[!UICONTROL Google Bigクエリに接続]**」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームに、名前、オプションの説明、BigQuery資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/google-big-query/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するBigQueryアカウントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![](../../../../images/tutorials/create/google-big-query/existing.png)

## 次の手順

このチュートリアルに従って、GBQアカウントへの接続を確立しました。 次のチュートリアルに進み、[データを [!DNL Platform]](../../dataflow/databases.md)に取り込むようにデータフローを設定できます。
