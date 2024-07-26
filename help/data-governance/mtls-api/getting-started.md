---
title: MTLS サービス API の概要
description: このドキュメントでは、MTLS API を正常に使用するために知っておく必要がある追加情報を提供します。
role: Developer
source-git-commit: f0b9d414d7b08015478c132de6910629d86c9cf9
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 42%

---

# MTLS サービス API の概要 {#getting-started}

MTLS サービス API を使用すると、Adobeによって発行された公開証明書を安全に取得および検証できます。

次の節では、MTLS サービス API を正しく使用するために必要な追加情報を示します。

## API 呼び出し例の読み取り

MTLS サービス API ドキュメントには、API 呼び出しの例とリクエストの形式を指定する方法が示されています。 これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。 また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

## 必須ヘッダー

また、API ドキュメントでは、Platform エンドポイントを正しく呼び出すために、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了している必要があります。次に示すように、Experience Platform API 呼び出しの必要な各ヘッダーの値は、認証に関するチュートリアルで説明されています。

- Authorization: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

## 次の手順

MTLS サービス API を使用して呼び出しを行うには、左側のナビゲーションを使用して、または [ 開発者ガイドの概要内で、エンドポイントガイドを選択 ](./overview.md) ます。
