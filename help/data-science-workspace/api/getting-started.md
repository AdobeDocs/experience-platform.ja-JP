---
keywords: Experience Platform;developer guide;endpoint;Data Science Workspace;popular topics
solution: Experience Platform
title: Senesi Machine Learning API開発者ガイド
topic: Developer guide
translation-type: tm+mt
source-git-commit: b0b44f4aaf365f58086cfa17d27fbba6ed2a2a97

---


# Senesi Machine Learning API開発者ガイド

Sensei Machine Learning APIは、データ科学者が機械学習サービスを整理、管理するメカニズムを提供します。これは、アルゴリズムのオンボーディングから実験まで、サービスの導入までを対象としています。

この開発者ガイドは、 [](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml)Sensei Machine Learning APIを使用して開始を行う際に役立つ手順を提供し、様々なData Science Workspaceリソースに対してCRUD操作を実行するためのAPI呼び出しの例を示します。

## はじめに

次のリクエストヘッダーにアクセスし [て](../../tutorials/authentication.md) 、Adobe Experience Platform APIを呼び出すには、認証チュートリアルを完了する必要があります。

* 認証：無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次のヘッダーが追加で必要です。

* コンテンツタイプ：application/json

## 次の手順

必要な認証資格情報を収集したら、次のエンドポイントグループへのサンプルAPI呼び出しについて、この開発者ガイドの後続の節に進むことができます。

* [エンジン](./engines.md)
* [実験](./experiments.md)
* [インサイト](./insights.md)
* [MLInstances（レシピ）](./mlinstances.md)
* [MLServices](./mlservices.md)
* [モデル](./models.md)
* [付録](./appendix.md)