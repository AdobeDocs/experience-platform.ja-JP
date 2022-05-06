---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
solution: Experience Platform
title: ソース SDK の概要（ベータ版）
topic-legacy: developer guide
description: このドキュメントでは、Sources SDK を使用して新しいソースを作成する前に知っておく必要がある前提条件の情報について説明します。
hide: true
hidefromtoc: true
exl-id: ba131442-ff20-4854-87fe-918aa313382d
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 57%

---

# ソース SDK の概要（ベータ版）

>[!IMPORTANT]
>
>Sources SDK は現在ベータ版です。お客様の組織はまだアクセスできない可能性があります。このドキュメントで説明されている機能は変更されることがあります。

ソース SDK を使用すると、独自の REST ベースのソースを統合して、データをAdobe Experience Platformに取り込むことができます。 このドキュメントでは、 [[!DNL Flow Service] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml).

## 前提条件

ソース SDK を使用するには、Adobe Experience Platform Sources でプロビジョニングされた IMS 組織サンドボックスにアクセスできることを確認する必要があります。

また、このガイドでは、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## API 呼び出し例の読み取り

ソース SDK および [!DNL Flow Service] API ドキュメントには、API 呼び出しの例が記載されており、リクエストの形式を設定する方法を示しています。 これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

## 必須ヘッダーの値の収集

Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

Platform のすべてのリソース ( [!DNL Flow Service]は、特定の仮想サンドボックスに分離されています。 Platform API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>Platform のサンドボックスについて詳しくは、 [サンドボックスドキュメント](../../../sandboxes/home.md).

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 次の手順

Sources SDK を使用して新しいソースの作成を開始するには、 [新しいソースの作成](./create.md).
