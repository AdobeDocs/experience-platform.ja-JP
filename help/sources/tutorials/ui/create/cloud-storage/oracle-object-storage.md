---
keywords: Experience Platform；ホーム；人気のトピック；Oracle オブジェクトストレージ；oracle オブジェクトストレージ
solution: Experience Platform
title: UI でのOracle オブジェクトストレージ Source接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して、Oracle オブジェクトストレージソース接続を作成する方法を説明します。
exl-id: 32284163-5dde-4171-8977-f76ceeebcef2
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 32%

---

# UI での [!DNL Oracle Object Storage] Source接続の作成

このチュートリアルでは、Adobe Experience Platform UI を使用して [!DNL Oracle Object Storage] ソース接続を作成する手順について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [&#x200B; ソース &#x200B;](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [&#x200B; サンドボックス &#x200B;](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### 必要な資格情報の収集

を [!DNL Oracle Object Storage] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `serviceUrl` | 認証に必要な [!DNL Oracle Object Storage] エンドポイント。 エンドポイントの形式は `https://{OBJECT_STORAGE_NAMESPACE}.compat.objectstorage.eu-frankfurt-1.oraclecloud.com` です。 |
| `accessKey` | 認証に必要な [!DNL Oracle Object Storage] アクセスキー ID。 |
| `secretKey` | 認証に必要な [!DNL Oracle Object Storage] パスワード。 |
| `bucketName` | ユーザーがアクセスを制限している場合に必要な、許可されたバケット名。 バケット名は 3 ～ 63 文字で、先頭と末尾は文字または数字である必要があります。また、小文字、数字、ハイフン （`-`）のみを含めることができます。 バケット名を IP アドレスのようにフォーマットすることはできません。 |
| `folderPath` | ユーザーがアクセスを制限している場合は、許可されたフォルダーパスが必要です。 |

これらの値の取得方法について詳しくは、[Oracle オブジェクトストレージ認証ガイド &#x200B;](https://docs.oracle.com/en-us/iaas/Content/Identity/Concepts/usercredentials.htm#User_Credentials) を参照してください。

必要な資格情報を収集したら、次の手順に従って、Experience Platformに接続するための新しいOracle オブジェクトストレージアカウントを作成できます。

## Oracle オブジェクトストレージへの接続

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、[!UICONTROL Sources] ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索バーを使用して、利用したい特定のソースを見つけることもできます。

[!UICONTROL &#x200B; クラウドストレージ &#x200B;] カテゴリで、「**[!UICONTROL Oracle オブジェクトストレージ]**」を選択し、**[!UICONTROL データを追加]** を選択します。

![カタログ](../../../../images/tutorials/create/oracle-object-storage/catalog.png)

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL Oracle Object Storage] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/oracle-object-storage/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新規アカウント]**」を選択し、続けて名前、説明（オプション）、[!DNL Oracle Object Storage] の資格情報を指定します。終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![新規](../../../../images/tutorials/create/oracle-object-storage/new.png)

## 次の手順

このチュートリアルでは、[!DNL Oracle Object Storage] アカウントとの接続を確立しました。次は、[&#x200B; クラウドストレージからExperience Platformにデータを取り込むためのデータフローの設定 &#x200B;](../../dataflow/batch/cloud-storage.md) に関するチュートリアルに進むことができます。
