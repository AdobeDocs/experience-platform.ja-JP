---
title: サンドボックスツール API ガイドの付録
description: このドキュメントでは、サンドボックスツール API の操作に関する補足情報を提供します。
exl-id: fdfa019d-ce0e-456b-b591-7d96d1115e02
source-git-commit: 308d07cf0c3b4096ca934a9008a13bf425dc30b6
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 2%

---

# サンドボックス API ガイドの付録

このドキュメントでは、 [!DNL Sandbox] API.

## クエリパラメーターの使用 {#query}

サンドボックスツール API では、パッケージをリストする際に、クエリパラメーターを使用して結果をページ付けおよびフィルタリングすることができます。

>[!NOTE]
>
>The `limit` は、個別に渡したり開始したりできます。

| パラメーター | 説明 |
| --- | --- |
| `limit` | 応答で返されるレコードの最大数。 デフォルトの制限は 20 です。 |
| `start` | サブセット化された項目リストの開始位置。 |
| `targetSandbox` | アーティファクトを読み込むサンドボックス名を表します。 |
| `jobType` | ジョブのタイプ。 この値は、NEW、RESUME、ROLLBACK のいずれかです。 |
| `jobStatus` | ジョブのステータス。 この値は、PENDING、IN_PROGRESS、SUCCESS、FAILED、CANCELLED のいずれかです。 |
| `requestType` | リクエストのタイプ。 この値は、EXPORT、IMPORT、COPY のいずれかです。 |
| `expiryPeriod ` | このユーザー指定の期間は、パッケージが公開された日時からのパッケージの有効期限（日数）を定義します。 この値は負にはできません。 デフォルト値は、更新または公開 API が呼び出されてから 90 日です。 |
