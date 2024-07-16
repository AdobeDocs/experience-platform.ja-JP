---
title: Privacy ServiceAPI ガイド
description: Privacy API を使用して、サポートされているAdobe Experience Cloud アプリケーションのPrivacy Serviceジョブをプログラムで管理する方法について説明します。
role: Developer
exl-id: 665466ac-2447-4a9d-a8cf-62092c09e431
source-git-commit: c16ce1020670065ecc5415bc3e9ca428adbbd50c
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 15%

---

# [!DNL Privacy Service] API ガイド

Privacy ServiceAPI は、組織のプライバシージョブをプログラムで管理できるいくつかのエンドポイントを提供します。 これらのエンドポイントの概要を以下に示します。詳しくは、個々のエンドポイントのガイドを参照し、必要なヘッダー、サンプル API 呼び出しの読み取りなどに関する重要な情報については、[はじめに](./getting-started.md)のガイドを参照してください。

>[!NOTE]
>
>このガイドでは、[!DNL Privacy Service] API の使用方法について説明します。 UI の使用方法について詳しくは、[Privacy Service UI の概要 ](../ui/overview.md) を参照してください。

使用可能なすべてのエンドポイントと CRUD 操作を表示するには、[Privacy ServiceAPI リファレンス ](https://www.adobe.io/experience-platform-apis/references/privacy-service/) にアクセスしてください。

## プライバシージョブ

Privacy Serviceは、サブジェクトの個人データに対するアクセスまたは削除のリクエストを受け取ると、そのリクエストを満たすためにプライバシージョブを作成します。 各プライバシージョブには、データ主体に関連する ID 情報、ジョブが適用されるAdobe Experience Cloud商品に関するメタデータ、ジョブの処理ステータスが含まれます。

`/jobs` エンドポイントを使用すると、組織のプライバシージョブを作成および取得できます。 このエンドポイントの使用方法については、[ プライバシージョブエンドポイントガイド ](./privacy-jobs.md) を参照してください。

## 同意

特定の規制では、個人データを収集する前に、顧客の明示的な同意が必要です。 `/consent` エンドポイントを使用すると、顧客同意リクエストを処理し、それらをプライバシーワークフローに統合できます。 詳しくは、[ 同意エンドポイントガイド ](./consent.md) を参照してください。

## 次の手順

エンドポイント API を使用して呼び出しを開始するには、[ はじめに ](./getting-started.md) ガイドを読み、エンドポイントガイドの 1 つを選択して、特定のPrivacy Serviceの使用方法を学習します。
