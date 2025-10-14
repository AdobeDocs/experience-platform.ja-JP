---
description: Destination SDKを使用するには、パートナー会社がこのドキュメントに記載されている前提条件を満たしている必要があります。
title: 統合の前提条件
exl-id: 031af9f1-ce18-4056-bd53-199ce8b56be5
source-git-commit: c1ba465a8a866bd8bdc9a2b294ec5d894db81e11
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 2%

---

# 統合の前提条件

Destination SDKを使用するには、以下の節に記載されている技術的およびパートナーシップの前提条件を満たしていることを確認してください。

## ストリーミング宛先の技術的および API の前提条件 {#streaming-prerequisites}

1. Adobe Experience Platformの REST API エンドポイントがあり、次のタイプのデータをに配信できます。
   * オーディエンスメンバーシップ情報
   * プロファイル ID 情報
   * （任意）プロファイルエンリッチメントの追加属性。
2. お使いの REST API エンドポイントは、基本、ベアラートークンまたは OAuth 2.0 認証プロトコルをサポートしています。
3. （任意）プログラムによるメタデータ管理用のオーディエンス作成/更新/削除 API または API エンドポイントがあります。

## バッチ宛先の技術的な前提条件 {#batch-prerequisites}

1. [!DNL Amazon S3]、[!DNL Azure Blob]、[!DNL Azure Data Lake Storage]、[!DNL SFTP]、[!DNL Google Cloud] またはプライベートフ [!DNL Data Landing Zone] ームでホストされた出力先があり、Experience Platformから書き出されたファイルを受け取ることができます。
2. 宛先プラットフォームは、バッチ宛先にDestination SDKして [&#x200B; ファイル形式オプション &#x200B;](functionality/destination-server/file-formatting.md) で設定された形式でファイルを取り込むことができます。
3. （任意）プログラムによるメタデータ管理用のオーディエンス作成/取得/更新/削除（[!DNL CRUD]） API または API エンドポイントがあります。

## パートナーシップの前提条件 {#partnership-prerequisites}

独立系ソフトウェアベンダー（ISV）またはシステムインテグレーター（SI）のDestination SDKの使用を検討している場合は、[&#x200B; アクセスの取得 &#x200B;](overview.md#get-access) の ISV と SI のパートナーシップ要件をお読みください。
