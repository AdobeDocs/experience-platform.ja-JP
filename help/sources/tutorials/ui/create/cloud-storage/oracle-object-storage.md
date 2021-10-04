---
keywords: Experience Platform；ホーム；人気のあるトピック；Oracleオブジェクトストレージ；oracleオブジェクトストレージ
solution: Experience Platform
title: UI でのOracleオブジェクトストレージソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して、Oracleオブジェクトストレージソース接続を作成する方法を説明します。
exl-id: 32284163-5dde-4171-8977-f76ceeebcef2
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 10%

---

# UI での [!DNL Oracle Object Storage] ソース接続の作成

このチュートリアルでは、Adobe Experience Platform UI を使用して [!DNL Oracle Object Storage] ソース接続を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

### 必要な資格情報の収集

で [!DNL Oracle Object Storage] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `serviceUrl` | 認証に必要な [!DNL Oracle Object Storage] エンドポイント。 エンドポイントの形式は次のとおりです。`https://{OBJECT_STORAGE_NAMESPACE}.compat.objectstorage.eu-frankfurt-1.oraclecloud.com` |
| `accessKey` | 認証に必要な [!DNL Oracle Object Storage] アクセスキー ID。 |
| `secretKey` | 認証に必要な [!DNL Oracle Object Storage] パスワード。 |
| `bucketName` | ユーザーがアクセスを制限している場合に必要な許可されたバケット名。 バケット名は 3～63 文字で、先頭と末尾は文字または数字で指定し、小文字、数字、ハイフン (`-`) のみを含める必要があります。 バケット名は、IP アドレスのような形式にはできません。 |
| `folderPath` | ユーザーがアクセスを制限している場合に必要な、許可されたフォルダーパス。 |

これらの値の取得方法の詳細については、『[Oracleオブジェクトストレージ認証ガイド ](https://docs.oracle.com/en-us/iaas/Content/Identity/Concepts/usercredentials.htm#User_Credentials)』を参照してください。

必要な資格情報を収集したら、次の手順に従って、新しいOracleオブジェクトストレージアカウントを作成し、Platform に接続します。

## oracleオブジェクトストレージに接続

Platform UI で、左のナビゲーションから「 **[!UICONTROL ソース]** 」を選択して、「 [!UICONTROL  ソース ] 」ワークスペースにアクセスします。 [!UICONTROL  カタログ ] 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索バーを使用して、作業対象の特定のソースを見つけることもできます。

「[!UICONTROL  クラウドストレージ ]」カテゴリで、「**[!UICONTROL Oracleオブジェクトストレージ]**」を選択し、「**[!UICONTROL データを追加]**」を選択します。

![カタログ](../../../../images/tutorials/create/oracle-object-storage/catalog.png)

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL Oracle Object Storage] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して次に進みます。

![既存](../../../../images/tutorials/create/oracle-object-storage/existing.png)

### 新規アカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、名前、説明（オプション）、[!DNL Oracle Object Storage] 資格情報を入力します。 終了したら、[**[!UICONTROL ソースに接続]**] を選択し、新しい接続が確立されるまでしばらく時間をかけます。

![新規](../../../../images/tutorials/create/oracle-object-storage/new.png)

## 次の手順

このチュートリアルに従って、[!DNL Oracle Object Storage] アカウントへの接続を確立しました。 次の [ データフローの設定に関するチュートリアルに進み、クラウドストレージから Platform](../../dataflow/batch/cloud-storage.md) にデータを取り込むことができます。
