---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;API;はじめに
solution: Experience Platform
title: アクセス制御 API ガイド
topic-legacy: developer guide
description: Adobe Experience Platform のアクセス制御では、Adobe Admin Console を使用して、様々な Platform 機能のロールと権限を管理できます。次の節では、Schema Registry API を正しく呼び出すために、開発者が知っておく必要がある追加情報を示します。
exl-id: 6fd956fb-ade4-48d3-843f-4c9a605945c9
source-git-commit: 2a73571d806f1653dad29d2c0b0067c5ce63e0e7
workflow-type: tm+mt
source-wordcount: '385'
ht-degree: 100%

---

# [!DNL Access Control] API ガイド

[!DNL Experience Platform] の [!DNL Access control] は、[Adobe Admin Console](https://adminconsole.adobe.com) でおこないます。この機能は、Admin Console の製品プロファイルを利用して、ユーザーを権限およびサンドボックスにリンクします。詳しくは、「[アクセス制御の概要](../home.md)」を参照してください。

この開発者ガイドでは、[[!DNL Access Control API]](https://www.adobe.io/experience-platform-apis/references/access-control/) へのリクエストのフォーマット方法に関する情報を提供し、次の操作について説明します。

- [権限名とリソースタイプのリスト](./permissions-and-resource-types.md)
- [現在のユーザーに対して効果の高いポリシーの表示](./effective-policies.md)

## はじめに

以下のセクションでは、[!DNL Schema Registry] API への呼び出しを正常に実行するために必要な追加情報を示しています。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{IMS_ORG}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name： `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type： application/json

## 次の手順

必要な資格情報を収集したので、開発者ガイドの残りをお読みください。各節では、エンドポイントに関する重要な情報を提供し、CRUD 操作を実行するための API 呼び出しの例を示します。各呼び出しには、一般的な API 形式、必要なヘッダーと適切な形式のペイロードを示すサンプルリクエスト、および正常な呼び 出しのサンプル応答が含まれます。
