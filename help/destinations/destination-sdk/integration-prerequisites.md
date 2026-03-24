---
description: Destination SDKを使用するには、パートナー企業がこのドキュメントに記載されている前提条件を満たしている必要があります。
title: 統合の前提条件
exl-id: 031af9f1-ce18-4056-bd53-199ce8b56be5
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 2%

---

# 統合の前提条件

Destination SDKを使用するには、以下のセクションに記載されている技術およびパートナーシップの前提条件を満たしていることを確認してください。

## ストリーミング宛先の技術的/API前提条件 {#streaming-prerequisites}

1. 次の種類のデータを配信するための[!DNL Adobe Experience Platform]のREST API エンドポイントがあります。
   * オーディエンスメンバーシップ情報；
   * プロファイル ID情報；
   * （オプション）プロファイルエンリッチメントの追加属性。
2. REST API エンドポイントは、基本、ベアラートークン、またはOAuth 2.0認証プロトコルをサポートしています。
3. （オプション）プログラマティックメタデータ管理用のオーディエンス作成/更新/削除APIまたはAPI エンドポイントがあります。

## バッチ宛先の技術的な前提条件 {#batch-prerequisites}

1. 宛先の場所は、[!DNL Amazon S3]、[!DNL Azure Blob]、[!DNL Azure Data Lake Storage]、[!DNL SFTP]、[!DNL Google Cloud]またはプライベート [!DNL Data Landing Zone]にホストされています。この場所では、Experience Platformから書き出されたファイルを受信できます。
2. 宛先プラットフォームは、バッチ配信先のDestination SDKの[ ファイル形式オプション ](functionality/destination-server/file-formatting.md)で設定された形式でファイルを取り込むことができます。
3. （オプション）プログラマティックメタデータ管理用のオーディエンス作成/取得/更新/削除（[!DNL CRUD]） APIまたはAPI エンドポイントがあります。

## パートナーシップの前提条件 {#partnership-prerequisites}

Independent Software Vendor （ISV）またはSystem Integrator （SI）でDestination SDKを使用する場合は、[ アクセスの取得の節](overview.md#get-access)でISVとSIのパートナーシップ要件を参照してください。
