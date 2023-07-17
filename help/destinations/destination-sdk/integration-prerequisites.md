---
description: Destination SDKを使用するには、パートナー会社がこのドキュメントに記載されている前提条件を満たす必要があります。
title: 統合の前提条件
exl-id: 031af9f1-ce18-4056-bd53-199ce8b56be5
source-git-commit: c1ba465a8a866bd8bdc9a2b294ec5d894db81e11
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 2%

---

# 統合の前提条件

Destination SDKを使用するには、以下の節に示す技術的およびパートナーシップの前提条件を満たしていることを確認してください。

## ストリーミング宛先の技術/ API の前提条件 {#streaming-prerequisites}

1. Adobe Experience Platformが次のタイプのデータをに配信するための REST API エンドポイントがある場合：
   * オーディエンスメンバーシップ情報
   * プロファイル ID 情報
   * （オプション）プロファイルエンリッチメント用の追加の属性。
2. REST API エンドポイントは、基本的な認証プロトコル、bearer トークン、または OAuth 2.0 認証プロトコルをサポートしています。
3. （オプション）プログラムでメタデータを管理するためのオーディエンスの API または API エンドポイントを作成、更新、削除できます。

## バッチ保存先の技術的な前提条件 {#batch-prerequisites}

1. ホスト先の場所があります [!DNL Amazon S3], [!DNL Azure Blob], [!DNL Azure Data Lake Storage], [!DNL SFTP], [!DNL Google Cloud]、またはプライベート [!DNL Data Landing Zone]:Experience Platform外に書き出されたファイルを受け取ることができます。
2. 宛先プラットフォームは、 [ファイル形式オプション](functionality/destination-server/file-formatting.md) バッチ保存先のDestination SDK。
3. （オプション）オーディエンスの作成、取得、更新、削除 ([!DNL CRUD]) プログラムメタデータ管理用の API または API エンドポイント。

## パートナーシップの前提条件 {#partnership-prerequisites}

Destination SDKの使用を検討している独立系ソフトウェアベンダー (ISV) またはシステムインテグレータ (SI) の場合は、 [アクセスの節を取得する](overview.md#get-access).
