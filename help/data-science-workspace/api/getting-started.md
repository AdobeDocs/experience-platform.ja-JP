---
keywords: Experience Platform；開発者ガイド；エンドポイント；Data Science Workspace；人気のあるトピック；Data Science Workspace;Data Science
solution: Experience Platform
title: Sensei 機械学習 API ガイド
topic-legacy: Developer guide
description: Sensei 機械学習 API を使用すると、開発者は様々な Data Science Workspace リソースに対して CRUD 操作を実行できます。 このガイドに従って、API を使用した主な操作の実行方法を学習します。
exl-id: d51d0eb2-b1e9-4cc1-889a-9487395703b0
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 74%

---

# [!DNL Sensei Machine Learning] API ガイド

[!DNL Sensei Machine Learning] API は、アルゴリズムのオンボーディングから実験まで、およびサービスの導入まで、データサイエンティストが機械学習サービスを整理および管理するためのメカニズムを提供します。

この開発者ガイドは、[Sensei 機械学習 API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml) の使用を開始する際に役立つ手順を提供し、様々な Data Science Workspace リソースに対して CRUD 操作を実行するための API 呼び出しの例を示します。

## はじめに

次のリクエストヘッダーにアクセスして API を呼び出すには、[認証](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)チュートリアルを完了しておく必要があります。[!DNL Adobe Experience Platform]

* Authorization： Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id： `{IMS_ORG}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name： `{SANDBOX_NAME}`

[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

* Content-Type： application/json

## 次の手順

必要な認証資格情報を収集したら、この開発者ガイドの後続の節に進んで、次のエンドポイントグループに属するサンプル API 呼び出しを参照することができます。

* [エンジン](./engines.md)
* [実験](./experiments.md)
* [Insights](./insights.md)
* [MLInstance（レシピ）](./mlinstances.md)
* [MLService](./mlservices.md)
* [モデル](./models.md)
* [付録](./appendix.md)
