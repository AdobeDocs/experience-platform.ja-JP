---
keywords: Experience Platform；ホーム；人気のトピック；Adobe Experience Platform;api ガイド；platform api ガイド；platform の概要；デベロッパーガイド
solution: Experience Platform
title: Adobe Experience Platform API の概要
description: Adobe Experience Platformは、相互に密接にリンクした API サービスを提供します。 このガイドには、使用可能なサービス、CRUD 操作に必要なヘッダー、エラーメッセージ、Postman コレクションおよびサンプル API 呼び出しに関する情報が含まれています。
role: Developer
feature: API
exl-id: a362bcb4-a908-43a8-abd3-0e1d21cb9117
source-git-commit: c0eb5b5c3a1968cae2bc19b7669f70a97379239b
workflow-type: tm+mt
source-wordcount: '1446'
ht-degree: 27%

---

# Adobe Experience Platform API の概要

Adobe Experience Platformは、「API ファースト」の理念の下に開発されています。 Platform API を使用すると、計算属性の設定、データやエンティティへのアクセス、データのエクスポート、不要なデータやバッチの削除など、データに対する基本的な CRUD （作成、読み取り、更新、削除）操作をプログラムで実行できます。

各Experience Platformサービスの API はすべて認証ヘッダーのセットを共有し、CRUD 操作に同様の構文を使用します。 次のガイドでは、Platform API の使用を開始するために必要な手順の概要を説明します。

## 認証とヘッダー

Platform エンドポイントを正常に呼び出すには、[ 認証チュートリアル ](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja) を完了する必要があります。 次に示すように、Experience Platform API 呼び出しの必要な各ヘッダーの値は、認証に関するチュートリアルで説明されています。

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {ORG_ID}`

### サンドボックスヘッダー

Experience Platform のすべてのリソースは、特定の仮想サンドボックスに分離されています。Platform API へのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- `x-sandbox-name: {SANDBOX_NAME}`

Platform のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../sandboxes/home.md)を参照してください。

### Content-type ヘッダー

リクエスト本文にペイロードを持つすべてのリクエスト（POST、PUT、PATCH 呼び出しなど）には、`Content-Type` ヘッダーが含まれている必要があります。指定できる値は、各 API エンドポイントによって異なります。 エンドポイントに特定の `Content-Type` 値が必要な場合、その値は、[ 個々の Platform サービスの API ガイド ](#api-guides) によって提供される API リクエストの例に表示されます。

## Experience PlatformAPI の基本事項

Adobe Experience Platform API では、Platform リソースを効果的に管理するために理解しておくべき、いくつかの基本テクノロジーおよび構文を使用します。

JSON スキーマオブジェクトの例など、Platform が利用する基盤となる API テクノロジーについて詳しくは、[Experience Platform API の基礎 ](api-fundamentals.md) ガイドを参照してください。

## Experience PlatformAPI 用のPostman コレクション

Postmanは、API 開発のコラボレーションプラットフォームで、プリセット変数の設定、API コレクションの共有、CRUD リクエストの効率化などを行うことができます。 ほとんどの Platform API サービスには、API 呼び出しの実行を支援するために使用できるPostman コレクションがあります。

環境の設定方法、使用可能なコレクションのリスト、コレクションの読み込み方法など、Postmanについて詳しくは、[Platform Postman ドキュメント ](postman.md) を参照してください。

## API 呼び出し例の読み取り {#sample-api}

リクエストの形式は、使用する Platform API によって異なります。API 呼び出しの構造を学ぶ最善の方法は、使用している特定の Platform サービスのドキュメントに記載されている例に従うことです。

[!DNL Experience Platform] のドキュメントは、2 つの異なる方法での API 呼び出しの例を示しています。 まず、呼び出しは **API 形式**&#x200B;で表されます。これは、操作（GET、POST、PUT、PATCH、DELETE など）と使用中のエンドポイント（例えば、`/global/classes`）のみを示すテンプレート表現です。また、テンプレートには、`GET /{VARIABLE}/classes/{ANOTHER_VARIABLE}` などの呼び出しの作成方法を示すために、変数の位置を示すものもあります。

その後、呼び出しは、**リクエスト**&#x200B;内の cURL コマンドとして表示されます。これには、API とのやり取りに必要なヘッダーと完全な「ベースパス」が含まれます。ベースパスは、すべてのエンドポイントの前に追加する必要があります。例えば、前述の `/global/classes` エンドポイントは `https://platform.adobe.io/data/foundation/schemaregistry/global/classes` になります。ドキュメント全体で API 形式/ リクエストパターンが表示され、Platform API に対して独自の呼び出しを行う際に、リクエスト例に示している完全なパスを使用することが想定されます。

### API リクエストの例

以下は、ドキュメントで使用される形式を示す API リクエストの例です。

**API 形式**

API 形式は、操作（GET）と使用されているエンドポイントを示します。変数は中括弧で示されます（この場合は `{CONTAINER_ID}`）。

```http
GET /{CONTAINER_ID}/classes
```

**リクエスト**

この例のリクエストでは、API 形式の変数には、リクエストパス内の実際の値が与えられます。さらに、必要なすべてのヘッダーは、サンプルヘッダー値または機密情報（セキュリティトークンやアクセス ID など）を含める必要がある変数として表示されます。

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

[Platform トラブルシューティングガイド ](troubleshooting.md#errors-and-troubleshooting) には、Experience Platformサービスの使用時に発生する可能性のあるエラーのリストが記載されています。

個々の Platform サービスのトラブルシューティングガイドについては、[ サービストラブルシューティングディレクトリ ](troubleshooting.md#service-troubleshooting-directory) を参照してください。

必要なヘッダーやリクエスト本文など、Platform API の特定のエンドポイントについて詳しくは、[Platform API ガイド ](#api-guides) を参照してください。

## Platform API ガイド {#api-guides}

| API ガイド | 説明 |
| --- | --- |
| [[!DNL Access Control] API ガイド ](.././access-control/api/getting-started.md) | [!DNL Access Control] API エンドポイントは、指定されたサンドボックス内の指定されたリソースに対して、ユーザーに有効な現在のポリシーを取得できます。 その他のすべてのアクセス制御機能は、[Adobe Admin Console](https://adminconsole.adobe.com/) を通じて提供されます。 |
| [ バッチ取得 API ガイド ](.././ingestion/batch-ingestion/api-overview.md) | Adobe Experience Platform [!DNL Data Ingestion] API を使用すると、データをバッチファイルとして Platform に取り込むことができます。 取り込まれるデータは、CRM システム内のフラットファイル（Parquet ファイルなど）のプロファイルデータや、スキーマレジストリ（XDM）内の既知のスキーマに準拠するデータの場合があります。 |
| [[!DNL Catalog Service] API ガイド ](.././catalog/api/getting-started.md) | [!DNL Catalog Service] API を使用すると、開発者はAdobe Experience Platformでデータセットメタデータを管理できます。 これには、データの場所、処理段階、処理中に発生したエラー、データレポートが含まれます。 |
| [[!DNL Data Access] API ガイド ](.././data-access/api.md) | [!DNL Data Access] API を使用すると、開発者はExperience Platform内で取り込んだデータセットに関する情報を取得できます。 これには、データセットファイルへのアクセスとダウンロード、ヘッダー情報の取得、失敗したバッチと成功したバッチのリスト、プレビュー CSV/Parquet ファイルのダウンロードが含まれます。 |
| [[!DNL Dataset Service] API ガイド ](.././data-governance/labels/dataset-api.md) | Dataset Service API を使用すると、データセットの使用ラベルを適用および編集できます。これは Adobe Experience Platform のデータカタログ機能の一部ですが、データセットメタデータを管理する Catalog Service API とは別のものです。 |
| [[!DNL Data Hygiene API guide]](../hygiene/api/overview.md) | [!DNL Data Hygiene] API を使用すると、Adobe Experience Platformに保存された顧客の個人データをプログラムで修正または削除したり、データセットの有効期限をスケジュール設定したりできます。 |
| [[!DNL Edge Network Server] API ガイド ](../server-api/overview.md) | [!DNL Edge Network Server API] は、様々なデータ収集、パーソナライゼーション、広告、マーケティングのユースケースに使用できます。 [!DNL Server API] は、サーバー、[!DNL IoT] デバイス、セットトップボックス、その他の様々なデバイスで使用できます。 |
| [[!DNL Identity Service] API ガイド ](.././identity-service/api/getting-started.md) | [!DNL Identity Service] API を使用すると、デベロッパーは、Adobe Experience Platformの ID グラフを使用して、クロスデバイス、クロスチャネル、ほぼリアルタイムでの顧客の ID を管理できます。 |
| [[!DNL MTLS Service API guide]](../data-governance/mtls-api/overview.md) | [!DNL MTLS Service] API を使用すると、Adobeが発行した組織の公開証明書を安全に取得できます。 |
| [[!DNL Observability Insights] API ガイド ](.././observability/api/overview.md) | [!DNL Observability Insights] は、デベロッパーがAdobe Experience Platformで主要な観察性指標を公開できるようにする RESTful API です。 これらの指標は、Platform の使用状況の統計、Platform サービスのヘルスチェック、様々な Platform 機能の過去の傾向とパフォーマンス指標に関する洞察を提供します。 |
| [[!DNL Policy Service] API ガイド ](.././data-governance/api/overview.md) <br> （データガバナンス） | [!DNL Policy Service] API を使用すると、データ使用ラベルとポリシーを作成および管理し、特定のデータ使用ラベルを含むデータに対して実行できるマーケティングアクションを決定できます。 データセットとフィールドにラベルを適用するには、[[!DNL Dataset Service] API](.././data-governance/labels/dataset-api.md) ガイドを参照してください。 |
| [[!DNL Privacy Service] API ガイド ](.././privacy-service/api/getting-started.md) | [!DNL Privacy Service] API を使用すると、デベロッパーは、法的なプライバシー規制に従って、Experience Cloudアプリケーションをまたいで個人データにアクセスしたり削除したりするカスタマーリクエストを作成および管理できます。 |
| [[!DNL Query Service] API ガイド ](.././query-service/api/getting-started.md) | [!DNL Query Service] API を使用すると、開発者は標準の SQL を使用してAdobe Experience Platform データに対してクエリを実行できます。 |
| [[!DNL Real-Time Customer Profile] API ガイド ](.././profile/api/overview.md) | リアルタイム顧客プロファイル API を使用すると、開発者は、プロファイルの表示、結合ポリシーの作成と更新、プロファイルデータのエクスポートやサンプリング、不要になったプロファイルデータやエラーで追加されたプロファイルデータの削除など、プロファイルデータを調査および操作できます。 |
| [Sandbox API ガイド ](.././sandboxes/api/getting-started.md) | Sandbox API を使用すると、開発者はAdobe Experience Platformの独立した仮想サンドボックス環境をプログラムで管理できます。 |
| [[!DNL Schema Registry] API ガイド ](.././xdm/api/overview.md) <br> （XDM） | [!DNL Schema Registry] API を使用すると、開発者はAdobe Experience Platform内のすべてのスキーマと関連する Experience Data Model （XDM）リソースをプログラムで管理できます。 |
| [[!DNL Segmentation Service] API ガイド ](.././segmentation/api/overview.md) | [!DNL Segmentation Service] API を使用すると、開発者はAdobe Experience Platformのセグメント化操作をプログラムで管理できます。 これには、リアルタイム顧客プロファイルデータからのセグメントの構築やオーディエンスの生成が含まれます。 |
| [[!DNL Sensei Machine Learning] API ガイド ](.././data-science-workspace/api/getting-started.md) <br> （Data Science Workspace） | [!DNL Sensei Machine Learning] API は、データサイエンティストが、アルゴリズムのオンボーディング、実験およびサービスのデプロイメントに至るまで、機械学習（ML）サービスを整理および管理するためのメカニズムを提供します。 |

各サービスで使用できる特定のエンドポイントと操作について詳しくは、Adobe I/Oの [API リファレンスドキュメント ](https://www.adobe.com/go/platform-api-reference-en) を参照してください。

## 次の手順

このドキュメントでは、必要なヘッダー、使用可能なガイドを紹介し、API 呼び出しの例を示しました。 Adobe Experience Platformで API 呼び出しを行うために必要なヘッダー値が揃ったので、調査する API エンドポイントを [Platform API ガイドの表 ](#api-guides) から選択します。

よくある質問への回答については、[Platform トラブルシューティングガイド ](troubleshooting.md) を参照してください。

Platform 環境を設定し、使用可能なPostman コレクションを調べるには、[Postman Postman ガイド ](postman.md) を参照してください。
