---
title: データハイジーン API ガイド
description: Adobe Experience Platform で顧客の保存した個人データをプログラムで修正または削除する方法を説明します。
exl-id: 78c8b15b-b433-4168-a1e8-c97b96e4bf85
source-git-commit: 22da9e39e168d9a995c7c134733aa7a1b3587749
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 100%

---

# データハイジーン API ガイド

>[!IMPORTANT]
>
>Adobe Experience Platform のデータハイジーン機能は、現在、Adobe Shield for Healthcare を購入した組織でのみ利用できます。

Data Hygiene API を使用すると、Adobe Experience Platform に保存された顧客の個人データをプログラムで修正または削除したり、データセットの Time-to-Live（TTL）プロトコルをスケジュール設定したりできます。このガイドでは、API を使用するための前提条件の手順を説明し、よりエンドポイントに特化したドキュメントへのリンクを提供します。

## はじめに

ルートパス `https://platform.adobe.io/data/core/hygiene/` を使用して、Data Hygiene API にアクセスできます。

これ以降の節では、API を呼び出す前に知っておく必要のある中心概念について説明します。

### 必須ヘッダーの値の収集

Data Hygiene API を呼び出すには、まず認証資格情報を収集する必要があります。[API 認証ガイド](../../landing/api-authentication.md)に従って、次に示すように、Data Hygiene API に必要な各ヘッダーの値を生成します。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次のような追加ヘッダーが必要です。

* `Content-Type: application/json`

### API 呼び出し例の読み取り

このドキュメントでは、リクエストの形式を示すために、API 呼び出しの例を提供しています。サンプル API 呼び出しのドキュメントで使用されている規則については、Experience Platform API の開始ガイドの[API 呼び出し例の読み方](../../landing/api-guide.md#sample-api)に関する節を参照してください。

<!-- ## Work orders

A work order is a representation of a data hygiene task that deletes consumer identities from a specific dataset or all datasets. See the [work order endpoint guide](./workorder.md) for details on working with work orders in the API. -->

## データセットの Time to Live（TTL）

データセット TTL は、時間差の「データセットの削除」アクションです。TTL を作成することで、そのデータセットが削除されるべき将来の時間を指定することになります。API でのデータセット TTL のスケジュール設定について詳しくは、[データセット TTL エンドポイントガイド](./ttl.md)を参照してください。

## 次の手順

このガイドでは、API 呼び出しを使用したデータハイジーンリクエストの管理方法について説明しました。Platform UI でこれらのアクションを実行する方法について詳しくは、[データハイジーン UI ガイド](../ui/overview.md)を参照してください。
