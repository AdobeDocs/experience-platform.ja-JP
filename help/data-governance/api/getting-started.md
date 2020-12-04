---
keywords: Experience Platform;home;popular topics;DULE;dule
solution: Experience Platform
title: Policy Service APIの使い始めに
topic: developer guide
description: Policy Service APIを使用すると、Adobe Experience Platformデータガバナンスに関する様々なリソースを作成および管理できます。 このドキュメントでは、ポリシーサービス API を呼び出す前に知っておく必要があるコア概念の概要を説明します。
translation-type: tm+mt
source-git-commit: 3376d6cace9ab196f457e2bf7b84cde06693103c
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 32%

---


# Getting started with the [!DNL Policy Service] API

この [!DNL Policy Service] APIを使用すると、Adobe Experience Platformに関連する様々なリソースを作成および管理でき [!DNL Data Governance]ます。 This document provides an introduction to the core concepts you need to know before attempting to make calls to the [!DNL Policy Service] API.

## 前提条件

開発者ガイドを使用するには、データガバナンス機能の操作に関わる様々な [!DNL Experience Platform] サービスについて、十分な理解が必要です。 Before beginning to work with the [!DNL Policy Service API], please review the documentation for the following services:

* [[!DNL Data Governance]](../home.md):データ使用のコンプライアンスを [!DNL Experience Platform] 適用するフレームワーク。
* [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 整理する際に使用される標準化されたフレームワーク。
* [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [サンドボックス](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

## API 呼び出し例の読み取り

The [!DNL Policy Service] API documentation provides example API calls to demonstrate how to format your requests. この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

## 必須ヘッダー

また、API ドキュメントでは、 エンドポイントを正しく呼び出すために、[認証に関するチュートリアル](../../tutorials/authentication.md)を完了している必要があります。[!DNL Platform]Completing the authentication tutorial provides the values for each of the required headers in [!DNL Experience Platform] API calls, as shown below:

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

All resources in [!DNL Experience Platform], including those belonging to [!DNL Data Governance], are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

* `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../sandboxes/home.md).

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## コアリソースとカスタムリソース

Within the [!DNL Policy Service] API, all policies and marketing actions are referred to as either `core` or `custom` resources.

`core` リソースとは、Adobeが定義および保守するものです。 `custom` リソースとは、組織が作成および保守するものであり、したがって、IMS組織に対してのみ一意で表示されます。 このため、`core` リソースに対して許可される操作は、リストと参照の操作（`GET`）のみです。一方、`custom` リソースに対しては、リスト、参照、および更新の操作（`POST`、`PUT`、`PATCH`、`DELETE`）を使用できます。

## 次の手順

Policy Service APIを使用した呼び出しを開始するには、使用可能なエンドポイントガイドの1つを選択します。