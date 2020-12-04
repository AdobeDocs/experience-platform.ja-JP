---
keywords: Experience Platform;home;popular topics;access control;api;getting started
solution: Experience Platform
title: アクセス制御開発者ガイド
topic: developer guide
description: Adobe Experience Platform のアクセス制御では、Adobe Admin Console を使用して、様々な Platform 機能のロールと権限を管理できます。以下の節では、Schema Registry API への呼び出しを正常に実行するために必要な追加情報を示しています。
translation-type: tm+mt
source-git-commit: 28b733a16b067f951a885c299d59e079f0074df8
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 67%

---


# [!DNL Access control] 開発ガイド

[!DNL Access control] というの [!DNL Experience Platform] は、 [Adobe Admin Consoleを通して投与されるからだ](https://adminconsole.adobe.com)。 この機能は、Admin Console の製品プロファイルを利用して、権限およびサンドボックスを持つユーザーをリンクします。詳しくは、「[アクセス制御の概要](../home.md)」を参照してください。

This developer guide provides information on how to format your requests to the [[!DNL Access Control API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/access-control.yaml), and covers the following operations:

- [権限名とリソースタイプの一覧表示](./permissions-and-resource-types.md)
- [現在のユーザーに対して効果の高いポリシーの表示](./effective-policies.md)

## はじめに

The following sections provide additional information that you will need to know in order to successfully make calls to the [!DNL Schema Registry] API.

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../sandboxes/home.md).

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: application/json

## 次の手順

必要な資格情報を収集したので、開発者ガイドの残りをお読みください。各節では、エンドポイントに関する重要な情報を提供し、CRUD 操作を実行するための API 呼び出しの例を示します。各呼び出しには、一般的な API 形式、必要なヘッダーと適切な形式のペイロードを示すサンプルリクエスト、および正常な呼び 出しのサンプル応答が含まれます。
