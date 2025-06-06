---
title: サンドボックスツール API の概要
description: サンドボックスツール API を使用して、アーティファクトを調査し、サンドボックス間でサンドボックス設定のスナップショットを書き出しと読み込みを行います。 このガイドに従って、API を使用した主な操作の実行方法を学習します。
exl-id: 0b34d153-a603-4397-a375-9cc846efe23a
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 63%

---

# サンドボックスツール API の概要 {#getting-started}

この開発者ガイドでは、サンドボックスツール API を使用してAdobe Experience Platformのパッケージとツールを管理するための手順を説明し、様々な操作を実行するためのサンプル API 呼び出しが含まれています。

## API 呼び出し例の読み取り {#api-calls}

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。API 応答に返されるサンプル JSON データも提供されます。 API 呼び出し例のドキュメントで使用される表記について詳しくは、『Experience Platform トラブルシューティングガイド』の [API 呼び出し例の読み方](/help/landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

## 必要なヘッダーの値の収集 {#headers}

このガイドでは、Experience Platform API を正しく呼び出すために、[ 認証に関するチュートリアル ](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja) を完了している必要があります。 認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

認証ヘッダーに加えて、すべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次の追加のヘッダーが必要です。

* `Content-Type: application/json`

## 次の手順 {#next-steps}

必要な資格情報を収集したので、開発者ガイドの残りをお読みください。各節では、エンドポイントに関する重要な情報を提供し、CRUD 操作を実行するための API 呼び出しの例を示します。各呼び出しには、一般的な API 形式、必要なヘッダーと適切な形式のペイロードを示すサンプルリクエスト、および正常な呼び 出しのサンプル応答が含まれます。

サンドボックスツール API への呼び出しを開始するには、次の API チュートリアルを参照してください。

* [パッケージエンドポイント](./packages.md)
* [ツールエンドポイント](./tools.md)
