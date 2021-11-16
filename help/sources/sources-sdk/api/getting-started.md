---
keywords: Experience Platform；ホーム；人気の高いトピック；ソース；コネクタ；ソースコネクタ；ソース sdk;SDK;SDK
solution: Experience Platform
title: ソース SDK の概要（ベータ版）
topic-legacy: developer guide
description: このドキュメントでは、Sources SDK を使用して新しいソースを作成する前に知っておく必要がある前提条件の情報について説明します。
hide: true
hidefromtoc: true
source-git-commit: d4b5b54be9fa2b430a3b45eded94a523b6bd4ef8
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 41%

---

# ソース SDK の概要（ベータ版）

>[!IMPORTANT]
>
>ソース SDK は現在ベータ版です。お客様の組織はまだアクセスできない可能性があります。 このドキュメントで説明する機能は、変更される場合があります。

ソース SDK を使用すると、独自の REST ベースのソースを統合して、データをAdobe Experience Platformに取り込むことができます。 このドキュメントでは、 [[!DNL Flow Service] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml).

## 前提条件

ソース SDK を使用するには、Adobe Experience Platform Sources でプロビジョニングされた IMS 組織サンドボックスにアクセスできることを確認する必要があります。

また、このガイドでは、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

## API 呼び出し例の読み取り

ソース SDK および [!DNL Flow Service] API ドキュメントには、API 呼び出しの例が記載されており、リクエストの形式を設定する方法を示しています。 これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

## 必須ヘッダーの値の収集

Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

Platform のすべてのリソース ( [!DNL Flow Service]は、特定の仮想サンドボックスに分離されています。 Platform API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>Platform のサンドボックスについて詳しくは、 [サンドボックスドキュメント](../../../sandboxes/home.md).

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 次の手順

Sources SDK を使用して新しいソースの作成を開始するには、 [新しいソースの作成](./create.md).