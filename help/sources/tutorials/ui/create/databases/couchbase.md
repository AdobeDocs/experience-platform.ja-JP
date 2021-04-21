---
keywords: Experience Platform；ホーム；人気のあるトピック；Couchbase;couchbase
solution: Experience Platform
title: UIでのCouchbaseソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してCouchbaseソース接続を作成する方法を説明します。
exl-id: 4270a48a-843c-4f1e-b280-35b620581d68
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '467'
ht-degree: 6%

---

# UIに[!DNL Couchbase]ソース接続を作成する

>[!NOTE]
>
> [!DNL Couchbase]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

[!DNL Adobe Experience Platform]のソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform]ユーザーインターフェイスを使用して[!DNL Couchbase]ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、[!DNL Platform]の次のコンポーネントについて十分に理解している必要があります。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効な[!DNL Couchbase]接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)のチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Couchbase]ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL Couchbase]インスタンスへの接続に使用する接続文字列です。 [!DNL Couchbase]の接続文字列パターンは`Server={SERVER}; Port={PORT};AuthMech=1;CredString=[{\"user\": \"{USER}\", \"pass\":\"{PASS}\"}];`です。 接続文字列の取得について詳しくは、[[!DNL Couchbase] connection](https://docs.Couchbase.com/c-sdk/2.10/client-settings.html#configuring-overview)のドキュメントを参照してください。 |

## [!DNL Couchbase]アカウントに接続

必要な資格情報を収集したら、次の手順に従って[!DNL Couchbase]アカウントを[!DNL Platform]にリンクします。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して&#x200B;**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

**[!UICONTROL Databases]**&#x200B;カテゴリーの下で、**[!UICONTROL Couchbase]**&#x200B;を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して新しい[!DNL Couchbase]コネクタを作成します。

![カタログ](../../../../images/tutorials/create/couchbase/catalog.png)

**[!UICONTROL Couchbaseに接続]**&#x200B;ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明、[!DNL Couchbase]資格情報を入力します。 終了したら、[**[!UICONTROL ソースに接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/couchbase/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する[!DNL Couchbase]アカウントを選択し、右上隅の「**[!UICONTROL 次へ]**」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/couchbase/existing.png)

## 次の手順

このチュートリアルに従うと、[!DNL Couchbase]アカウントへの接続が確立されます。 次のチュートリアルに進み、[データを [!DNL Platform]](../../dataflow/databases.md)に取り込むようにデータフローを設定できます。
