---
keywords: Experience Platform；ホーム；人気のトピック；属性ベースのアクセス制御；属性ベースのアクセス制御
title: 属性ベースのアクセス制御 API の概要
description: 属性ベースのアクセス制御 API を使用すると、Adobe Experience Platform内のロールとアクセスポリシーをプログラムによって管理できます。 このガイドに従って、API を使用した主な操作の実行方法を学習します。
role: Developer
exl-id: d1a66afa-dff4-49d7-b57c-527f05977155
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '328'
ht-degree: 63%

---

# 属性ベースのアクセス制御 API の概要

このデベロッパーガイドでは、属性ベースのアクセス制御 API を使用して、Adobe Experience Platformのロール、プロダクト、権限カテゴリおよび権限セットを管理する手順を説明します。また、様々な操作を実行するためのサンプル API 呼び出しが含まれています。

## API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。API 呼び出し例のドキュメントで使用される表記について詳しくは、『Experience Platform トラブルシューティングガイド』の [API 呼び出し例の読み方](../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

## 必要なヘッダーの値の収集

このガイドでは、Experience Platform API を正しく呼び出すために、[&#x200B; 認証に関するチュートリアル &#x200B;](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja) を完了している必要があります。 認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

認証ヘッダーに加えて、すべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次の追加のヘッダーが必要です。

* `Content-Type: application/json`

## 次の手順

必要な資格情報を収集したので、開発者ガイドの残りをお読みください。各節では、エンドポイントに関する重要な情報を提供し、CRUD 操作を実行するための API 呼び出しの例を示します。各呼び出しには、一般的な API 形式、必要なヘッダーと適切な形式のペイロードを示すサンプルリクエスト、および正常な呼び 出しのサンプル応答が含まれます。

属性ベースのアクセス制御 API の呼び出しを開始するには、次の API チュートリアルを参照してください。

* [役割エンドポイント](./roles.md)
* [製品エンドポイント](./products.md)
