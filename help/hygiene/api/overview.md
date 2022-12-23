---
title: データハイジーン API ガイド
description: Adobe Experience Platform で顧客の保存した個人データをプログラムで修正または削除する方法を説明します。
exl-id: 78c8b15b-b433-4168-a1e8-c97b96e4bf85
source-git-commit: da8b5d9fffdf8a176a4d70be5df5b3021cf0df7b
workflow-type: tm+mt
source-wordcount: '469'
ht-degree: 74%

---

# データハイジーン API ガイド

>[!IMPORTANT]
>
>Adobe Experience Platform のデータハイジーン機能は現在、**Adobe Healthcare Shield** または **Adobe Privacy &amp; Security Shield** を購入した組織でのみご利用いただけます。

Data Hygiene API を使用すると、Adobe Experience Platform に保存された顧客の個人データをプログラムで修正または削除したり、データセットの有効期限をスケジュール設定したりできます。このガイドでは、API を使用するための前提条件の手順を説明し、よりエンドポイントに特化したドキュメントへのリンクを提供します。

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

## データセット有効期限

データセットの有効期限は、時間差の「データセットの削除」アクションです。データセットの有効期限を作成すると、そのデータセットが削除されるべき将来の時間を指定することになります。API でのデータセット有効期限のスケジュール設定について詳しくは、[データセット有効期限のエンドポイントガイド](./dataset-expiration.md)を参照してください。

## レコードの削除

>[!IMPORTANT]
>
>レコードの削除リクエストは、を購入した組織でのみ使用できます **Adobeヘルスケアシールド**.
>
>
>レコードの削除は、データのクレンジング、匿名データの削除、またはデータの最小化に使用するためのものです。 これらは **not** :EU 一般データ保護規則 (GDPR) などのプライバシー規制に関するデータ主体の権利要求（コンプライアンス）に使用されます。 すべてのコンプライアンスの使用例に対して、 [Adobe Experience Platform Privacy Service](../../privacy-service/home.md) 代わりに、

データ衛生 API を使用すると、1 つまたはすべてのデータセットにわたって、ID に関連付けられたすべてのレコードを削除できます。 ID を削除するすべてのデータ衛生タスクは、作業指示と呼ばれる構成体によって表されます。 詳しくは、 [作業順序エンドポイントガイド](./workorder.md) を参照してください。

## 割り当て量

ユーザーの組織は、データハイジーン操作のタイプごとに、事前に定められた 1 か月のジョブの割り当て量が制限されています。この制限は、ライセンスに応じて異なる場合があります。データハイジーンプロセスの現在の割り当て量のステータスを表示する方法について詳しくは、[割り当て量のエンドポイントガイド](./quota.md)を参照してください。

## 次の手順

このガイドでは、API 呼び出しを使用したデータハイジーンリクエストの管理方法について説明しました。Platform UI でこれらのアクションを実行する方法について詳しくは、[データハイジーン UI ガイド](../ui/overview.md)を参照してください。
