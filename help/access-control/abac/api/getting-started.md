---
keywords: Experience Platform；ホーム；人気の高いトピック；属性ベースのアクセス制御；属性ベースのアクセス制御
title: 属性ベースのアクセス制御の概要
description: 属性ベースのアクセス制御を使用すると、Adobe Experience Platform内の役割とポリシーをプログラムで管理できます。 このガイドに従って、API を使用した主な操作の実行方法を学習します。
hide: true
hidefromtoc: true
exl-id: d1a66afa-dff4-49d7-b57c-527f05977155
source-git-commit: 19f1e8df8cd8b55ed6b03f80e42810aefd211474
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 64%

---

# 属性ベースのアクセス制御の概要

>[!IMPORTANT]
>
>現在、米国ベースの医療関連のお客様向けに、属性ベースのアクセス制御が限定的なリリースで提供されています。 この機能は、完全にリリースされると、すべてのReal-time Customer Data Platformのお客様が利用できるようになります。

この開発者ガイドは、属性ベースのアクセス制御を使用してAdobe Experience Platformの役割、製品、権限カテゴリおよび権限セットを管理する手順を提供し、様々な操作を実行するための API 呼び出し例を含んでいます。

## API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。API 呼び出し例のドキュメントで使用される表記について詳しくは、『Experience Platform トラブルシューティングガイド』の [API 呼び出し例の読み方](../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

## 必要なヘッダーの値の収集

このガイドでは、Platform API を正しく呼び出すために[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了している必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

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
