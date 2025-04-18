---
solution: Experience Platform
title: 統合タグ API の概要
description: 次のドキュメントでは、統合タグ API を正常に使用するために必要な追加情報を示しています。
role: Developer
exl-id: 8f33707f-b46d-4054-802c-9e42ecabd9ba
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 36%

---

# 統合タグ API の概要 {#getting-started}

Unified Tags API を使用すると、Adobe Experience Platform内のビジネスオブジェクトを分類および管理できます。

次の節では、統合タグ API を正しく使用するために必要な追加情報を示します。

## API 呼び出し例の読み取り

統合タグ API ドキュメントには、API 呼び出しの例とリクエストの形式を指定する方法が示されています。 これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの [API 呼び出し例の読み方 ](../../landing/troubleshooting.md#how-do-i-format-an-api-request) に関する節を参照してください。

## 必須ヘッダー

また、API ドキュメントでは、Experience Platform エンドポイントを正しく呼び出すために、[ 認証に関するチュートリアル ](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja) を完了している必要があります。 次に示すように、Experience Platform API 呼び出しの必要な各ヘッダーの値は、認証に関するチュートリアルで説明されています。

- Authorization: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Experience Platform] API へのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Experience Platform] でのサンドボックスの使用について詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

## 次の手順

統合タグ API を使用して呼び出しを行うには、左側のナビゲーションを使用して、または [ 開発者ガイドの概要内で、使用可能なエンドポイントガイドの 1 つを選択 ](./overview.md) ます。
