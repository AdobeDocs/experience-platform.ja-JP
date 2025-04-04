---
keywords: Experience Platform；開発者ガイド；エンドポイント；Data Science Workspace；人気のトピック；data science workspace;data science
solution: Experience Platform
title: Sensei機械学習 API ガイド
description: Sensei Machine Learning API を使用すると、開発者は様々な Data Science Workspace リソースに対して CRUD 操作を実行できます。 このガイドに従って、API を使用した主な操作の実行方法を学習します。
role: Developer
exl-id: d51d0eb2-b1e9-4cc1-889a-9487395703b0
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 56%

---

# [!DNL Sensei Machine Learning] API ガイド

>[!NOTE]
>
>Data Science Workspaceは購入できなくなりました。
>
>このドキュメントは、Data Science Workspaceの以前の使用権限を持つ既存のお客様を対象としています。

[!DNL Sensei Machine Learning] API は、データサイエンティストが、アルゴリズムのオンボーディングから実験、サービスのデプロイメントに至るまで、機械学習サービスを整理および管理するためのメカニズムを提供します。

この開発者ガイドは、[Sensei 機械学習 API](https://developer.adobe.com/experience-platform-apis/references/sensei-machine-learning/) の使用を開始する際に役立つ手順を提供し、様々な Data Science Workspace リソースに対して CRUD 操作を実行するための API 呼び出しの例を示します。

## はじめに

API を呼び出すために次のリクエストヘッダーにアクセスするには、[authentication](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja) チュートリアルを完了している必要 [!DNL Adobe Experience Platform] あります。

* Authorization： Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Experience Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name：`{SANDBOX_NAME}`

[!DNL Experience Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次のような追加ヘッダーが必要です。

* Content-Type：application/json

## 次の手順

必要な認証資格情報を収集したら、この開発者ガイドの後続の節に進んで、次のエンドポイントグループに属するサンプル API 呼び出しを参照することができます。

* [エンジン](./engines.md)
* [実験](./experiments.md)
* [Insights](./insights.md)
* [MLInstances （レシピ）](./mlinstances.md)
* [MLServices](./mlservices.md)
* [モデル](./models.md)
* [付録](./appendix.md)
