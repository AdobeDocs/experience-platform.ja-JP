---
keywords: Experience Platform;home;popular topics;identity service api;identity service developer guide;region
solution: Experience Platform
title: はじめに
topic: API guide
description: 'Adobe Experience Platform ID サービスは、Adobe Experience Platform 内の ID グラフと呼ばれる、デバイス間、チャネル間、およびほぼリアルタイムでの顧客の識別を管理します。 '
translation-type: tm+mt
source-git-commit: fa667d86c089c692f22cfd1b46f3f11b6e9a68d7
workflow-type: tm+mt
source-wordcount: '742'
ht-degree: 66%

---


# [!DNL Identity Service] API開発者ガイド

Adobe Experience Platform [!DNL Identity Service] manages the cross-device, cross-channel, and near real-time identification of your customers in what is known as an identity graph within Adobe Experience Platform.

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

- [[!DNL Identity Service]](../home.md):お客様のプロファイルデータの断片化に伴う基本的な課題を解決します。 これは、顧客がブランドと対話するデバイスやシステム間で ID を橋渡しするすることで実現します。
- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):顧客体験データを [!DNL Platform] 整理する際に使用される標準化されたフレームワーク。

The following sections provide additional information that you will need to know or have on-hand in order to successfully make calls to the [!DNL Identity Service] API.

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

### 地域ベースのルーティング

The [!DNL Identity Service] API employs region-specific endpoints that require the inclusion of a `{REGION}` as part of the request path. IMS 組織のプロビジョニング中に、地域が決定され、IMS Org プロファイル内に保存されます。Using the correct region with each endpoint ensures that all requests made using the [!DNL Identity Service] API are routed to the appropriate region.

There are two regions currently supported by [!DNL Identity Service] APIs: VA7 and NLD2.

次の表に、地域を使用したパスの例を示します。

| サービス | 地域：VA7 | 地域：NLD2 |
| ------ | -------- |--------- |
| [!DNL Identity Service] API | https://</span>platform-va7.adobe.</span>io/data/core/identity/{ENDPOINT} | https://</span>platform-nld2.adobe.</span>io/data/core/identity/{ENDPOINT} |
| [!DNL Identity Namespace] API | https://</span>platform-va7.adobe.</span>io/data/core/idnamespace/{ENDPOINT} | https://</span>platform-nld2.adobe.</span>io/data/core/idnamespace{ENDPOINT} |

>[!NOTE]
>
>地域を指定しないでリクエストをおこなうと、誤った地域に対する呼び出しルーティングが発生したり、呼び出しが予期せず失敗したりする場合があります。

IMS Org プロファイル内で地域が見つからない場合は、システム管理者に問い合わせてください。

## Using the [!DNL Identity Service] API

これらのサービスで使用される ID パラメーターは、次の 2 つの方法、複合または XID のいずれかで表現できます。

複合 ID は、ID 値と名前空間の両方を含む構成です。複合 ID を使用する場合、名前空間は名前（`namespace.code`）または ID（`namespace.id`）で指定できます。

When an identity is persisted, [!DNL Identity Service] generates and assigns an ID to that identity, called the native ID, or XID. クラスター API とマッピング API のすべてのバリエーションは、リクエストと応答で複合 ID と XID の両方をサポートしています。これらのAPIを使用するには、次のパラメーターのいずれかが必要です。`xid`、[`ns`または`nsid`]および`id`の組み合わせ。

応答のペイロードを制限するため、API は、使用される ID 構文のタイプに応じて、それらの応答を調整します。つまり、XID を渡すと応答に XID が含まれ、複合 ID を渡すと、応答はリクエストで使用される構造に従います。

The examples in this document do not cover the complete functionality of the [!DNL Identity Service] API. 完全な API については、「[Swagger API リファレンス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/id-service-api.yaml)」を参照してください。

>[!NOTE]
>
>リクエストでネイティブ XID が使用される場合、返される ID はすべてネイティブ XID 形式になります。ID／名前空間フォームの使用詳しくは、「[ID の XID の取得](./create-custom-namespace.md)」に関する節を参照してください。

## 次の手順

必要な資格情報を収集したので、開発者ガイドの残りをお読みください。各節では、エンドポイントに関する重要な情報を提供し、CRUD 操作を実行するための API 呼び出しの例を示します。各呼び出しには、一般的な API 形式、必要なヘッダーと適切な形式のペイロードを示すサンプルリクエスト、および正常な呼び 出しのサンプル応答が含まれます。
