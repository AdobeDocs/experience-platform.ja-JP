---
title: データハイジーン API ガイド
description: Adobe Experience Platform で顧客の保存した個人データをプログラムで修正または削除する方法を説明します。
role: Developer
exl-id: 78c8b15b-b433-4168-a1e8-c97b96e4bf85
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 69%

---

# データハイジーン API ガイド

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

## レコード削除

>[!IMPORTANT]
>
>レコードの削除リクエストは、**Adobe Healthcare Shield** を購入した組織でのみ利用できます。
>
>
>レコードの削除は、データクレンジング、匿名データの削除、またはデータの最小化のために使用されます。これらは、EU 一般データ保護規則（GDPR）などのプライバシー規制に関するデータサブジェクト権利リクエスト（コンプライアンス）に対して使用するためのものでは&#x200B;**ありません**。コンプライアンスに関するユースケースについて詳しくは、[Adobe Experience Platform Privacy Service](../../privacy-service/home.md) を参照してください。

Data Hygiene API を使用すると、1 つまたはすべてのデータセットにわたって、ID に関連付けられたすべてのレコードを削除できます。 ID を削除するすべてのデータライフサイクルタスクは、作業指示と呼ばれる構成によって表されます。 API でのレコード削除の操作について詳しくは、[ 作業指示エンドポイントガイド ](./workorder.md) を参照してください。

## 割り当て量

組織は、データライフサイクル操作のタイプごとに、事前に定義された 1 か月のジョブの割り当て量が制限されています。この制限は、ライセンスに応じて異なる場合があります。 データライフサイクルプロセスの現在の割り当て量のステータスを表示する方法について詳しくは、[ 割り当て量のエンドポイントガイド ](./quota.md) を参照してください。

## 次の手順

このガイドでは、API 呼び出しを使用したデータライフサイクルリクエストの管理方法について説明しました。 Experience Platform UI でこれらのアクションを実行する方法について詳しくは、[ データライフサイクル UI ガイド ](../ui/overview.md) を参照してください。
