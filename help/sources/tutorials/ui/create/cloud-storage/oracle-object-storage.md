---
keywords: Experience Platform；ホーム；人気のトピック；Oracleオブジェクトストレージ；oracleオブジェクトストレージ
solution: Experience Platform
title: UI でのOracleオブジェクトストレージソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用してOracleオブジェクトストレージソース接続を作成する方法を説明します。
exl-id: 32284163-5dde-4171-8977-f76ceeebcef2
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 46%

---

# の作成 [!DNL Oracle Object Storage] UI のソース接続

このチュートリアルでは、 [!DNL Oracle Object Storage] Adobe Experience Platform UI を使用したソース接続

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### 必要な認証情報の収集

接続先： [!DNL Oracle Object Storage]を使用する場合、次の接続プロパティの値を指定する必要があります。

| 認証情報 | 説明 |
| ---------- | ----------- |
| `serviceUrl` | この [!DNL Oracle Object Storage] 認証に必要なエンドポイント。 エンドポイントの形式は次のとおりです。 `https://{OBJECT_STORAGE_NAMESPACE}.compat.objectstorage.eu-frankfurt-1.oraclecloud.com` |
| `accessKey` | この [!DNL Oracle Object Storage] 認証に必要なアクセスキー ID。 |
| `secretKey` | この [!DNL Oracle Object Storage] 認証に必要なパスワード。 |
| `bucketName` | ユーザーがアクセスを制限している場合に必要な許可されたバケット名です。 バケット名は、3～63 文字で、先頭と末尾には文字または数字を使用し、小文字、数字、ハイフン (`-`) をクリックします。 バケット名は IP アドレスと同じ形式にできません。 |
| `folderPath` | ユーザーがアクセスを制限している場合に許可されるフォルダーパス。 |

これらの値の取得方法について詳しくは、 [Oracleオブジェクトストレージ認証ガイド](https://docs.oracle.com/en-us/iaas/Content/Identity/Concepts/usercredentials.htm#User_Credentials).

必要な資格情報を収集したら、次の手順に従って、新しいOracleオブジェクトストレージアカウントを作成し、Platform に接続します。

## oracleオブジェクトストレージに接続

Platform の UI で、左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索バーを使用して、利用したい特定のソースを見つけることもできます。

以下 [!UICONTROL クラウドストレージ] カテゴリ、選択 **[!UICONTROL Oracleオブジェクトストレージ]** 次に、 **[!UICONTROL データを追加]**.

![カタログ](../../../../images/tutorials/create/oracle-object-storage/catalog.png)

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL Oracle Object Storage] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/oracle-object-storage/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新規アカウント]**」を選択し、続けて名前、説明（オプション）、[!DNL Oracle Object Storage] の認証情報を指定します。終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![新規](../../../../images/tutorials/create/oracle-object-storage/new.png)

## 次の手順

このチュートリアルでは、[!DNL Oracle Object Storage] アカウントとの接続を確立しました。次のチュートリアル ( [データフローを設定して、クラウドストレージから Platform にデータを取り込む](../../dataflow/batch/cloud-storage.md).
