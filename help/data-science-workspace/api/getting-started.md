---
keywords: Experience Platform;developer guide;endpoint;Data Science Workspace;popular topics
solution: Experience Platform
title: Senesi Machine Learning API開発者ガイド
topic: Developer guide
translation-type: tm+mt
source-git-commit: 83e74ad93bdef056c8aef07c9d56313af6f4ddfd
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 2%

---


# Senesi Machine Learning API開発者ガイド

Senesie Machine Learning APIは、データ科学者が機械学習サービスを編成、管理するメカニズムを提供します。これは、アルゴリズムによる搭乗から実験まで、サービスの導入までを対象としています。

この開発者ガイドでは、 [](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml)Senesi Machine Learning APIを使用する開始を支援する手順を説明し、様々なData Science Workspaceリソースに対してCRUD操作を実行するAPI呼び出しの例を示します。

## はじめに

APIを呼び出すために次のリクエストヘッダーにアクセスするには、 [認証](../../tutorials/authentication.md)[!DNL Adobe Experience Platform] のチュートリアルを完了している必要があります。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されています。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

プラットフォームのサンドボックスについて詳しくは、「 [サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

* Content-Type: application/json

## 次の手順

必要な認証資格情報を収集したら、次のエンドポイントグループへのサンプルAPI呼び出しについて、この開発者ガイドの後続の節に進むことができます。

* [エンジン](./engines.md)
* [実験](./experiments.md)
* [インサイト](./insights.md)
* [MLInstances（レシピ）](./mlinstances.md)
* [MLServices](./mlservices.md)
* [モデル](./models.md)
* [付録](./appendix.md)