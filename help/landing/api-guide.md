---
keywords: Experience Platform；ホーム；人気の高いトピック；Adobe Experience Platform;api ガイド；platform api ガイド；platform の概要；開発者ガイド
solution: Experience Platform
title: Getting started with Adobe Experience Platform APIs
topic-legacy: api guide
description: Adobe Experience Platformは、相互に密接にリンクされた API サービスを提供します。 このガイドには、使用可能なサービス、CRUD 操作に必要なヘッダー、エラーメッセージ、Postmanコレクション、サンプル API 呼び出しに関する情報が含まれています。
exl-id: a362bcb4-a908-43a8-abd3-0e1d21cb9117
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '1379'
ht-degree: 30%

---

# Adobe Experience Platform API の概要

Adobe Experience Platformは、「API ファースト」の理念の下で開発されています。 Platform API を使用すると、計算済み属性の設定、データ/エンティティへのアクセス、データのエクスポート、不要なデータやバッチの削除など、データに対する基本的な CRUD（作成、読み取り、更新、削除）操作をプログラムで実行できます。

The APIs for each Experience Platform service all share the same set of authentication headers and use similar syntaxes for their CRUD operations. 以下のガイドでは、Platform API の使用を開始するために必要な手順の概要を説明します。

## 認証とヘッダー

In order to successfully make calls to Platform endpoints, you are required to complete the [authentication tutorial](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja). 次に示すように、Experience Platform API 呼び出しの必要な各ヘッダーの値は、認証に関するチュートリアルで説明されています。

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {ORG_ID}`

### サンドボックスヘッダー

Experience Platform のすべてのリソースは、特定の仮想サンドボックスに分離されています。Platform API へのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- `x-sandbox-name: {SANDBOX_NAME}`

Platform のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../sandboxes/home.md)を参照してください。

### Content-type ヘッダー

リクエスト本文にペイロードを持つすべてのリクエスト（POST、PUT、PATCH 呼び出しなど）には、`Content-Type` ヘッダーが含まれている必要があります。指定できる値は、各 API エンドポイントに固有です。 If a specific `Content-Type` value is needed for an endpoint, its value will be shown in the example API requests provided by the [API guides for individual Platform services](#api-guides).

## Experience Platform API の基本

Adobe Experience Platform API は、Platform リソースを効果的に管理するために理解する必要がある、基盤となる複数のテクノロジーと構文を使用しています。

JSON スキーマオブジェクトの例など、Platform が利用する基になる API テクノロジーの詳細については、 [Experience PlatformAPI の基本](api-fundamentals.md) ガイド。

## Postman collections for Experience Platform APIs

Postmanは、API 開発のコラボレーションプラットフォームで、プリセット変数を使用して環境を設定し、API コレクションを共有し、CRUD リクエストを合理化するなどのことができます。 Most Platform API services have Postman collections which can be used to assist with making API calls.

環境の設定方法、使用可能なコレクションのリスト、コレクションの読み込み方法など、Postmanについて詳しくは、 [Platform Postman documentation](postman.md).

## API 呼び出し例の読み取り {#sample-api}

リクエストの形式は、使用する Platform API によって異なります。API 呼び出しの構造を学ぶ最善の方法は、使用している特定の Platform サービスのドキュメントに記載されている例に従うことです。

のドキュメント [!DNL Experience Platform] に、2 つの異なる方法での API 呼び出し例を示します。 まず、呼び出しは **API 形式**&#x200B;で表されます。これは、操作（GET、POST、PUT、PATCH、DELETE など）と使用中のエンドポイント（例えば、`/global/classes`）のみを示すテンプレート表現です。また、テンプレートには、`GET /{VARIABLE}/classes/{ANOTHER_VARIABLE}` などの呼び出しの作成方法を示すために、変数の位置を示すものもあります。

その後、呼び出しは、**リクエスト**&#x200B;内の cURL コマンドとして表示されます。これには、API とのやり取りに必要なヘッダーと完全な「ベースパス」が含まれます。ベースパスは、すべてのエンドポイントの前に追加する必要があります。例えば、前述の `/global/classes` エンドポイントは `https://platform.adobe.io/data/foundation/schemaregistry/global/classes` になります。API 形式/リクエストパターンはドキュメント全体で確認できます。独自で Platform API を呼び出す場合は、例に示すリクエストに示す完全パスを使用する必要があります。

### API リクエストの例

以下は、ドキュメントで使用される形式を示す API リクエストの例です。

**API 形式**

API 形式は、操作（GET）と使用されているエンドポイントを示します。変数は中括弧で示されます（この場合は `{CONTAINER_ID}`）。

```http
GET /{CONTAINER_ID}/classes
```

**リクエスト**

この例のリクエストでは、API 形式の変数には、リクエストパス内の実際の値が与えられます。さらに、必要なすべてのヘッダーは、サンプルのヘッダー値、または機密情報（セキュリティトークンやアクセス ID など）を含める必要のある変数として表示されます。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/global/classes \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

この応答は、送信されたリクエストに基づいて、API の呼び出しが成功した後に何を受け取るかを示します。場合によっては、応答がスペースを節約するために切り捨てられいるため、サンプルに表示されている情報に加えて他の情報が表示されることがあります。

```json
{
    "results": [
        {
            "title": "XDM ExperienceEvent",
            "$id": "https://ns.adobe.com/xdm/context/experienceevent",
            "meta:altId": "_xdm.context.experienceevent",
            "version": "1"
        },
        {
            "title": "XDM Individual Profile",
            "$id": "https://ns.adobe.com/xdm/context/profile",
            "meta:altId": "_xdm.context.profile",
            "version": "1"
        }
    ],
    "_links": {}
}
```

## エラーメッセージ

この [Platform トラブルシューティングガイド](troubleshooting.md#errors-and-troubleshooting) は、任意のエラーサービスを使用する際に発生する可能性のあるExperience Platformのリストを提供します。

個々の Platform サービスのトラブルシューティングガイドについては、 [サービストラブルシューティングディレクトリ](troubleshooting.md#service-troubleshooting-directory).

必要なヘッダーやリクエスト本文を含む、Platform API の特定のエンドポイントについて詳しくは、 [Platform API ガイド](#api-guides).

## Platform API guides {#api-guides}

| API ガイド | 説明 |
| --- | --- |
| [[!DNL Access Control] API ガイド](.././access-control/api/getting-started.md) | この [!DNL Access Control] API エンドポイントは、指定されたサンドボックス内の特定のリソースに関して、ユーザーに対して有効な現在のポリシーを取得できます。 その他のすべてのアクセス制御機能は、 [Adobe Admin Console](https://adminconsole.adobe.com/). |
| [バッチ取得 API ガイド](.././ingestion/batch-ingestion/api-overview.md) | ザAdobe Experience Platform [!DNL Data Ingestion] API を使用すると、データをバッチファイルとして Platform に取り込むことができます。 CRM システムのフラットファイルのプロファイルデータ（Parquet ファイルなど）、またはスキーマレジストリ (XDM) の既知のスキーマに適合するデータを取り込むことができます。 |
| [[!DNL Catalog Service] API ガイド](.././catalog/api/getting-started.md) | The [!DNL Catalog Service] API allows developers to manage dataset metadata in Adobe Experience Platform. This includes data locations, processing stages, errors that occurred during processing, and data reports. |
| [[!DNL Data Access] API ガイド](.././data-access/api.md) | The [!DNL Data Access] API allows developers to retrieve information on ingested datasets within Experience Platform. これには、データセットファイルのアクセスとダウンロード、ヘッダー情報の取得、失敗したバッチと成功したバッチのリスト表示、CSV プレビュー/Parquet ファイルのダウンロードが含まれます。 |
| [[!DNL Dataset Service] API ガイド](.././data-governance/labels/dataset-api.md) | Dataset Service API を使用すると、データセットの使用ラベルを適用および編集できます。これは Adobe Experience Platform のデータカタログ機能の一部ですが、データセットメタデータを管理する Catalog Service API とは別のものです。 |
| [[!DNL Identity Service] API ガイド](.././identity-service/api/getting-started.md) | この [!DNL Identity Service] API を使用すると、開発者は、Adobe Experience Platformの ID グラフを使用して、デバイス間、チャネル間、ほぼリアルタイムでの顧客の識別を管理できます。 |
| [[!DNL Observability Insights] API ガイド](.././observability/api/overview.md) | [!DNL Observability Insights] は、開発者がAdobe Experience Platformで主要な観察性指標を公開できる RESTful API です。 これらの指標は、Platform の使用状況の統計、Platform サービスのヘルスチェック、様々な Platform 機能の過去の傾向とパフォーマンス指標に関する洞察を提供します。 |
| [[!DNL Policy Service] API guide](.././data-governance/api/overview.md) <br> (Data Governance) | この [!DNL Policy Service] API を使用すると、データ使用ラベルとポリシーを作成および管理して、特定のデータ使用ラベルを含むデータに対して実行できるマーケティングアクションを決定できます。 データセットとフィールドにラベルを適用するには、 [[!DNL Dataset Service] API](.././data-governance/labels/dataset-api.md) ガイド |
| [[!DNL Privacy Service] API ガイド](.././privacy-service/api/getting-started.md) | The [!DNL Privacy Service] API allows developers to create and manage customer requests to access or delete their personal data across Experience Cloud applications, in compliance with legal privacy regulations. |
| [[!DNL Query Service] API ガイド](.././query-service/api/getting-started.md) | この [!DNL Query Service] 開発者は、API を使用して、標準の SQL を使用してAdobe Experience Platformデータに対してクエリを実行できます。 |
| [[!DNL Real-time Customer Profile] API ガイド](.././profile/api/overview.md) | リアルタイム顧客プロファイル API を使用すると、プロファイルの表示、結合ポリシーの作成と更新、プロファイルデータの書き出しとサンプリング、不要になったまたはエラーで追加されたプロファイルデータの削除など、プロファイルデータの調査と操作ができます。 |
| [サンドボックス API ガイド](.././sandboxes/api/getting-started.md) | サンドボックス API を使用すると、開発者は、Adobe Experience Platformで分離された仮想サンドボックス環境をプログラムで管理できます。 |
| [[!DNL Schema Registry] API guide](.././xdm/api/overview.md) <br> (XDM) | この [!DNL Schema Registry] API を使用すると、開発者はAdobe Experience Platform内のすべてのスキーマと関連する Experience Data Model(XDM) リソースをプログラムで管理できます。 |
| [[!DNL Segmentation Service] API ガイド](.././segmentation/api/overview.md) | The [!DNL Segmentation Service] API allows developers to programmatically manage segmentation operations in Adobe Experience Platform. This includes building segments and generating audiences from your Real-time Customer Profile data. |
| [[!DNL Sensei Machine Learning] API ガイド](.././data-science-workspace/api/getting-started.md) <br> (Data Science Workspace) | この [!DNL Sensei Machine Learning] API は、データサイエンティストが、アルゴリズムのオンボーディング、実験、サービスのデプロイメントに至るまで、機械学習 (ML) サービスを整理および管理するメカニズムを提供します。 |

For more information on specific endpoints and operations available for each service, please see the [API reference documentation](https://www.adobe.com/go/platform-api-reference-en) on Adobe I/O.

## 次の手順

このドキュメントでは、必要なヘッダー、利用可能なガイド、API 呼び出しの例を紹介しました。 Adobe Experience Platformで API 呼び出しをおこなうために必要な必須のヘッダー値が揃ったので、 [Platform API ガイドの表](#api-guides).

よくある質問に対する回答については、 [Platform トラブルシューティングガイド](troubleshooting.md).

Postman環境を設定し、使用可能なPostmanコレクションを確認するには、 [Platform Postmanガイド](postman.md).
