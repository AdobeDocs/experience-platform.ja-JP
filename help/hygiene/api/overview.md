---
title: データ衛生 API ガイド
description: Adobe Experience Platform で顧客の保存した個人データをプログラムで修正または削除する方法を説明します。
exl-id: 78c8b15b-b433-4168-a1e8-c97b96e4bf85
hide: true
hidefromtoc: true
source-git-commit: c2e7cf1859f6a2b277783cdec535ecc208703fac
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 29%

---

# データ衛生 API ガイド

>[!IMPORTANT]
>
>現在、Adobe Experience Platformのデータ衛生機能は、医療用Adobeシールドを購入した組織でのみ使用できます。

データ衛生 API を使用すると、Adobe Experience Platformに保存された顧客の個人データをプログラムで修正または削除したり、データセットの有効期間 (TTL) プロトコルをスケジュールしたりできます。 このガイドでは、API を使用するための前提条件の手順を説明し、よりエンドポイントに固有のドキュメントへのリンクを提供します。

## はじめに

データ衛生 API には、次のルートパスからアクセスできます。 `https://platform.adobe.io/data/core/hygiene/`

以下の節では、API を呼び出す前に知っておく必要があるコア概念の概要を説明します。

### 必須ヘッダーの値の収集

データ衛生 API を呼び出すには、まず認証資格情報を収集する必要があります。フォロー： [API 認証ガイド](../../landing/api-authentication.md) 次に示すように、データ衛生 API に必要な各ヘッダーの値を生成します。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

### API 呼び出し例の読み取り

このドキュメントでは、リクエストの形式を示すために、API 呼び出しの例を提供しています。サンプル API 呼び出しのドキュメントで使用されている規則については、Experience Platform API の開始ガイドの[API 呼び出し例の読み方](../../landing/api-guide.md#sample-api)に関する節を参照してください。

## 作業指示

作業順序は、特定のデータセットまたはすべてのデータセットから消費者 ID を削除するデータ衛生タスクを表します。 詳しくは、 [作業順序エンドポイントガイド](./workorder.md) を参照してください。

## データセットの有効期間 (TTL)

データセット TTL は、時間遅延の「データセットの削除」アクションです。 TTL を作成することで、そのデータセットを削除する将来の時刻を指定します。 詳しくは、 [データセット TTL エンドポイントガイド](./ttl.md) を参照してください。

## 次の手順

このガイドでは、API 呼び出しを使用してデータの衛生要求を管理する方法について説明しました。 Platform UI でこれらのアクションを実行する方法について詳しくは、 [データ衛生 UI ガイド](../ui/overview.md).
