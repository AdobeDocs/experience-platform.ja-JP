---
keywords: Experience Platform;developer guide;endpoint;Data Science Workspace;popular topics
solution: Experience Platform
title: Senesi Machine Learning API開発者ガイド
topic: Developer guide
translation-type: tm+mt
source-git-commit: c48079ba997a7b4c082253a0b2867df76927aa6d
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 3%

---


# [!DNL Sensei Machine Learning] API開発者ガイド

この [!DNL Sensei Machine Learning] APIは、データ科学者が機械学習サービスを、アルゴリズムのオンボーディングから実験まで、サービスの展開まで、整理および管理するためのメカニズムを提供します。

この開発者ガイドでは、 [](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml)Senesi Machine Learning APIを使用する開始を支援する手順を説明し、様々なData Science Workspaceリソースに対してCRUD操作を実行するAPI呼び出しの例を示します。

## はじめに

APIを呼び出すために次のリクエストヘッダーにアクセスするには、 [認証](../../tutorials/authentication.md)[!DNL Adobe Experience Platform] のチュートリアルを完了している必要があります。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

* x-sandbox-name: `{SANDBOX_NAME}`

のサンドボックスについて詳し [!DNL Platform]くは、 [Sandboxの概要ドキュメントを参照してください](../../sandboxes/home.md)。

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