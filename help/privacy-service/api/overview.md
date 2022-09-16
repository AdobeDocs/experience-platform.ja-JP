---
title: Privacy ServiceAPI ガイド
description: Privacy ServiceAPI を使用して、サポートされるAdobe Experience Cloudアプリケーションのプライバシージョブをプログラムで管理する方法について説明します。
exl-id: 665466ac-2447-4a9d-a8cf-62092c09e431
source-git-commit: bda8d0ee1db4b58b4b856a23a8790cd7f76c0656
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 21%

---

# [!DNL Privacy Service] API ガイド

Privacy ServiceAPI は、組織のプライバシージョブをプログラムで管理できるエンドポイントをいくつか提供します。 これらのエンドポイントの概要を以下に示します。詳しくは、個々のエンドポイントのガイドを参照し、必要なヘッダー、サンプル API 呼び出しの読み取りなどに関する重要な情報については、[はじめに](./getting-started.md)のガイドを参照してください。

>[!NOTE]
>
>このガイドでは、 [!DNL Privacy Service] API UI の使用方法について詳しくは、「[Privacy Service サービス UI の概要](../ui/overview.md)」を参照してください。

使用可能なすべてのエンドポイントと CRUD の操作を表示するには、 [Privacy ServiceAPI リファレンス](https://www.adobe.io/experience-platform-apis/references/privacy-service/).

## プライバシージョブ

Privacy Serviceは、主体の個人データへのアクセスまたは削除のリクエストを受け取ると、そのリクエストを満たすためにプライバシージョブを作成します。 各プライバシージョブには、データ主体に関する ID 情報、ジョブが適用されるAdobe Experience Cloud製品に関するメタデータ、ジョブの処理ステータスが含まれます。

この `/jobs` endpoint を使用すると、組織のプライバシージョブを作成および取得できます。 このエンドポイントの使用方法については、 [privacy jobs endpoint guide](./privacy-jobs.md).

## 同意

特定の規制では、個人データを収集する前に、明示的なお客様の同意が必要です。 この `/consent` endpoint を使用すると、顧客の同意リクエストを処理して、プライバシーワークフローに統合できます。 詳しくは、 [同意エンドポイントガイド](./consent.md) を参照してください。

## 次の手順

Privacy ServiceAPI を使用した呼び出しを開始するには、 [入門ガイド](./getting-started.md) 次に、エンドポイントガイドの 1 つを選択して、特定のエンドポイントの使用方法を学びます。
