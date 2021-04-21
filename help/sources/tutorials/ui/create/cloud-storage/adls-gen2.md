---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure Data LakeストレージGen2;ADLS Gen2;adls gen2;adlsコネクタ
solution: Experience Platform
title: UIにAzure Data LakeストレージGen2ソース接続を作成する
topic-legacy: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してAzure Data LakeストレージGen2ソース接続を作成する方法を説明します。
exl-id: d81b7593-08a3-43f8-a8bc-f5547a6cd55a
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '484'
ht-degree: 9%

---

# UIに[!DNL Azure Data Lake Storage Gen2]ソース接続を作成する

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、[!DNL Azure Data Lake Storage Gen2] （以下「[!DNL ADLS Gen2]」と呼びます）ソースコネクタを[!DNL Platform]ユーザーインターフェイスを使用して認証する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効なADLS Gen2接続をお持ちの場合は、このドキュメントの残りの部分をスキップし、[データフローの設定](../../dataflow/batch/cloud-storage.md)のチュートリアルに進むことができます。

### 必要な資格情報の収集

[!DNL ADLS Gen2]ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `url` | [!DNL ADLS Gen2]のエンドポイントです。 |
| `servicePrincipalId` | アプリケーションのクライアントID。 |
| `servicePrincipalKey` | アプリのキー。 |
| `tenant` | アプリケーションを含むテナント情報。 |

これらの値の詳細については、[this [!DNL ADLS Gen2] ドキュメント](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-storage)を参照してください。

## [!DNL ADLS Gen2]アカウントに接続

必要な資格情報を収集したら、次の手順に従って[!DNL ADLS Gen2]アカウントをリンクして[!DNL Platform]に接続します。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して&#x200B;**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

**[!UICONTROL データベース]**&#x200B;カテゴリの下で、**[!UICONTROL Azure Data Lake Gen2]**&#x200B;を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して、新しいADLS Gen2コネクタを作成します。

![](../../../../images/tutorials/create/adls-gen2/catalog.png)

**[!UICONTROL Azure Data Lake Gen2]**&#x200B;に接続ダイアログが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明、[!DNL ADLS Gen2]資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![](../../../../images/tutorials/create/adls-gen2/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する[!DNL ADLS Gen2]アカウントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![](../../../../images/tutorials/create/adls-gen2/existing.png)

## 次の手順

このチュートリアルに従うと、[!DNL ADLS Gen2]アカウントへの接続が確立されます。 次のチュートリアルに進み、[データフローを設定して、クラウドストレージのデータを [!DNL Platform]](../../dataflow/batch/cloud-storage.md)に取り込むことができます。
