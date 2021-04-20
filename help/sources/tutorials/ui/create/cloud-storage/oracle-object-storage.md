---
keywords: Experience Platform；ホーム；人気のあるトピック；Oracleオブジェクトストレージ;oracleオブジェクトストレージ
solution: Experience Platform
title: UIでのOracleオブジェクトストレージソース接続の作成
topic: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してOracleオブジェクトストレージのソース接続を作成する方法を説明します。
translation-type: tm+mt
source-git-commit: c1453a9f0be42f834d35af871051324df8dadf80
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 10%

---


# UIでの[!DNL Oracle Object Storage]ソース接続の作成

このチュートリアルでは、Adobe Experience PlatformUIを使用して[!DNL Oracle Object Storage]ソース接続を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込むことができ、Platform Servicesを使用して、データの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

### 必要な資格情報の収集

[!DNL Oracle Object Storage]に接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `serviceUrl` | [!DNL Oracle Object Storage]エンドポイントは認証に必要です。 エンドポイントの形式は次のとおりです。`https://{OBJECT_STORAGE_NAMESPACE}.compat.objectstorage.eu-frankfurt-1.oraclecloud.com` |
| `accessKey` | 認証に必要な[!DNL Oracle Object Storage]アクセスキーIDです。 |
| `secretKey` | 認証に必要な[!DNL Oracle Object Storage]パスワードです。 |
| `bucketName` | ユーザーが制限付きアクセス権を持つ場合に必要な許可されたバケット名。 バケット名は3 ～ 63文字で、先頭と末尾は文字または数字で、小文字、数字、ハイフン(`-`)のみを含める必要があります。 バケット名は、IPアドレスと同じように形式設定できません。 |
| `folderPath` | ユーザーが制限付きアクセス権を持つ場合に必要な許可されたフォルダーパスです。 |

これらの値の取得方法の詳細については、『[Oracleオブジェクトストレージ認証ガイド](https://docs.oracle.com/en-us/iaas/Content/Identity/Concepts/usercredentials.htm#User_Credentials)』を参照してください。

必要な資格情報を収集したら、次の手順に従って新しいOracleオブジェクトストレージアカウントを作成し、プラットフォームに接続します。

## Oracleオブジェクトに接続ストレージ

プラットフォームUIで、左側のナビゲーションから「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索バーを使用して、使用する特定のソースを見つけることもできます。

[!UICONTROL クラウドストレージ]カテゴリーの下で、**[!UICONTROL Oracleオブジェクトストレージ]**&#x200B;を選択し、**[!UICONTROL 追加data]**&#x200B;を選択します。

![カタログ](../../../../images/tutorials/create/oracle-object-storage/catalog.png)

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する[!DNL Oracle Object Storage]アカウントを選択し、**[!UICONTROL 次へ]**&#x200B;を選択して次に進みます。

![既存の](../../../../images/tutorials/create/oracle-object-storage/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、名前、オプションの説明、および[!DNL Oracle Object Storage]資格情報を入力します。 終了したら、[**[!UICONTROL ソースに接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![新規](../../../../images/tutorials/create/oracle-object-storage/new.png)

## 次の手順

このチュートリアルに従うと、[!DNL Oracle Object Storage]アカウントへの接続が確立されます。 次の[データフローの設定に関するチュートリアルに進み、クラウドストレージのデータをプラットフォーム](../../dataflow/batch/cloud-storage.md)に取り込むことができます。
