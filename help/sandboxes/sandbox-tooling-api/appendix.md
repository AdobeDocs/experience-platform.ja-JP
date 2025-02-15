---
title: サンドボックスツール API ガイドの付録
description: このドキュメントでは、サンドボックスツール API の操作に関する補足情報を提供します。
exl-id: fdfa019d-ce0e-456b-b591-7d96d1115e02
source-git-commit: 955c6946786e9425bdb99d623595420a6d13747e
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 2%

---

# サンドボックス API ガイドの付録

このドキュメントでは、[!DNL Sandbox] API の操作に関する補足情報を提供します。

## クエリパラメーターの使用 {#query}

サンドボックスツール API では、パッケージをリストする際に、クエリパラメーターを使用してページ番号を付け、結果をフィルタリングできます。

>[!NOTE]
>
>`limit` は、個別に渡して開始できます。

| パラメーター | 説明 |
| --- | --- |
| `limit` | 応答で返されるレコードの最大数。 デフォルトの制限は 20 です。 |
| `start` | 項目のサブセット化されたリストを開始する場所の開始。 |
| `targetSandbox` | アーティファクトを読み込むサンドボックス名を表します。 |
| `jobType` | ジョブのタイプ。 この値には、`NEW`、`RESUME` または `ROLLBACK` を指定できます。 |
| `jobStatus` | ジョブのステータス。 この値には、`PENDING`、`IN_PROGRESS`、`SUCCESS`、`FAILED` または `CANCELLED` を指定できます。 |
| `requestType` | リクエストのタイプ。 この値には、`EXPORT`、`IMPORT` または `COPY` を指定できます。 |
| `expiryPeriod` | このユーザー指定の期間は、パッケージが公開された時点からのパッケージの有効期限（日数）を定義します。 この値は負の値にはできません。 デフォルト値は、更新または公開 API が呼び出されてから 90 日です。 |
| `packageType` | パッケージのタイプ。 この値には、`PARTIAL` または `FULL` を指定できます。 |
| `status` | パッケージのステータス。 この値には、`DRAFT`、`PUBLISHED`、`PUBLISH_IN_PROGRESS`、`PUBLISH_FAILED` を指定できます。 |
