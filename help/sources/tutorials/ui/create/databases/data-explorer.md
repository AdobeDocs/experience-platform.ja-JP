---
keywords: Experience Platform；ホーム；人気のあるトピック；AzureData Explorer;Azure Data Explorer;Data Explorer;Data Explorer
solution: Experience Platform
title: UIでAzureData Explorerソースコネクタを作成する
topic: overview
type: Tutorial
description: このチュートリアルでは、Platformユーザーインターフェイスを使用してAzureData Explorer(以下「Data Explorer」と呼ばれる)ソースコネクタを作成する手順を説明します。
translation-type: tm+mt
source-git-commit: f86f7483e7e78edf106ddd34dc825389dadae26a
workflow-type: tm+mt
source-wordcount: '519'
ht-degree: 8%

---


# UIに[!DNL Azure Data Explorer]ソースコネクタを作成する

>[!NOTE]
>
> [!DNL Azure Data Explorer]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform]ユーザーインターフェイスを使用して[!DNL Azure Data Explorer] （以下「[!DNL Data Explorer]」と呼びます）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効な[!DNL Data Explorer]接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)のチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL Platform]の[!DNL Data Explorer]アカウントにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `endpoint` | [!DNL Data Explorer]サーバーのエンドポイント。 |
| `database` | [!DNL Data Explorer]データベースの名前。 |
| `tenant` | [!DNL Data Explorer]データベースへの接続に使用する一意のテナントID。 |
| `servicePrincipalId` | [!DNL Data Explorer]データベースへの接続に使用する一意のサービスプリンシパルID。 |
| `servicePrincipalKey` | [!DNL Data Explorer]データベースへの接続に使用する一意のサービスプリンシパルキーです。 |

開始方法の詳細については、[この [!DNL Data Explorer] ドキュメント](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/access-control/how-to-authenticate-with-aad)を参照してください。

## [!DNL Azure Data Explorer]アカウントに接続

必要な資格情報を収集したら、次の手順に従って[!DNL Data Explorer]アカウントを[!DNL Platform]にリンクします。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して&#x200B;**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

**[!UICONTROL データベース]**&#x200B;カテゴリーの下で、**[!UICONTROL AzureData Explorer]**&#x200B;を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して新しいData Explorerコネクタを作成します。

![カタログ](../../../../images/tutorials/create/data-explorer/catalog.png)

**[!UICONTROL AzureData Explorerに接続]**&#x200B;ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明、[!DNL Data Explorer]資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/data-explorer/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する[!DNL Data Explorer]アカウントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![既存の](../../../../images/tutorials/create/data-explorer/existing.png)

## 次の手順

このチュートリアルに従うと、[!DNL Data Explorer]アカウントへの接続が確立されます。 次のチュートリアルに進み、[データを [!DNL Platform]](../../dataflow/databases.md)に取り込むようにデータフローを設定できます。