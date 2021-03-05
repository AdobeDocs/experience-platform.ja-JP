---
keywords: Experience Platform；ホーム；人気の高いトピック；Adobe Experience Platform;apiガイド；プラットフォームapiガイド；プラットフォームの紹介；開発者ガイド
solution: Experience Platform
title: Adobe Experience PlatformAPIの使い始めに
topic: apiガイド
description: Adobe Experience Platformは互いに密接に関連したAPIサービスを提供しています。 このガイドには、使用可能なサービス、CRUD操作に必要なヘッダー、エラーメッセージ、Postmanコレクション、サンプルAPI呼び出しに関する情報が含まれています。
translation-type: tm+mt
source-git-commit: 85d2ae5ccf1b27baaafe839f1d3f00d588abe4fc
workflow-type: tm+mt
source-wordcount: '1427'
ht-degree: 25%

---


# Adobe Experience PlatformAPIの使い始めに

Adobe Experience Platformは「APIファースト」という哲学の下で開発された。 プラットフォームAPIを使用すると、計算済み属性の設定、データ/エンティティへのアクセス、データの書き出し、不要なデータやバッチの削除など、データに対して基本的なCRUD（作成、読み取り、更新、削除）操作をプログラムで実行できます。

各Experience PlatformサービスのAPIは、すべて同じ認証ヘッダーのセットを共有し、CRUD操作に同様の構文を使用します。 以下のガイドは、プラットフォームAPIの使用を開始するために必要な手順の概要を説明しています。

## 認証とヘッダー

プラットフォームエンドポイントの呼び出しを正しく行うには、[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。 次に示すように、Experience Platform API 呼び出しの必要な各ヘッダーの値は、認証に関するチュートリアルで説明されています。

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {IMS_ORG}`

### Sandboxヘッダー

Experience Platform のすべてのリソースは、特定の仮想サンドボックスに分離されています。Platform API へのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- `x-sandbox-name: {SANDBOX_NAME}`

Platform のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../sandboxes/home.md)を参照してください。

### コンテンツタイプのヘッダー

リクエスト本文にペイロードを持つすべてのリクエスト（POST、PUT、PATCH 呼び出しなど）には、`Content-Type` ヘッダーが含まれている必要があります。指定できる値は、各APIエンドポイントに固有です。 エンドポイントに特定の`Content-Type`値が必要な場合、その値は、個々のプラットフォームサービス](#api-guides)の[APIガイドによって提供されるAPI要求の例に表示されます。

## Experience Platform API の基本

Adobe Experience PlatformのAPIは、プラットフォームリソースを効果的に管理するために理解する必要がある、基礎となるテクノロジーと構文をいくつか採用しています。

JSONスキーマオブジェクトの例を含め、Platformが利用する基盤となるAPIテクノロジーの詳細については、[Experience PlatformAPIの基本事項](api-fundamentals.md)ガイドを参照してください。

## Experience PlatformAPI用のPostmanコレクション

PostmanはAPI開発のコラボレーションプラットフォームで、プリセット変数を使用した環境の設定、APIコレクションの共有、CRUDリクエストの合理化などを可能にします。 ほとんどのプラットフォームAPIサービスにはPostmanコレクションがあり、API呼び出しの作成に役立てるのに使用できます。

環境の設定方法、使用可能なコレクションのリスト、コレクションの読み込み方法など、Postmanの詳細については、[Platform Postmanドキュメント](postman.md)を参照してください。

## API 呼び出し例の読み取り {#sample-api}

リクエストの形式は、使用する Platform API によって異なります。API 呼び出しの構造を学ぶ最善の方法は、使用している特定の Platform サービスのドキュメントに記載されている例に従うことです。

[!DNL Experience Platform]のドキュメントには、2つの異なる方法でAPI呼び出しの例が示されています。 まず、呼び出しは **API 形式**&#x200B;で表されます。これは、操作（GET、POST、PUT、PATCH、DELETE など）と使用中のエンドポイント（例えば、`/global/classes`）のみを示すテンプレート表現です。また、テンプレートには、`GET /{VARIABLE}/classes/{ANOTHER_VARIABLE}` などの呼び出しの作成方法を示すために、変数の位置を示すものもあります。

その後、呼び出しは、**リクエスト**&#x200B;内の cURL コマンドとして表示されます。これには、API とのやり取りに必要なヘッダーと完全な「ベースパス」が含まれます。ベースパスは、すべてのエンドポイントの前に追加する必要があります。例えば、前述の `/global/classes` エンドポイントは `https://platform.adobe.io/data/foundation/schemaregistry/global/classes` になります。ドキュメント全体にAPI形式/リクエストパターンが表示され、独自のプラットフォームAPIを呼び出す場合は、この例のリクエストに示されている完全なパスを使用する必要があります。

### API リクエストの例

以下は、ドキュメントで使用される形式を示す API リクエストの例です。

**API 形式**

API 形式は、操作（GET）と使用されているエンドポイントを示します。変数は中括弧で示されます（この場合は `{CONTAINER_ID}`）。

```http
GET /{CONTAINER_ID}/classes
```

**リクエスト**

この例のリクエストでは、API 形式の変数には、リクエストパス内の実際の値が与えられます。また、必要なすべてのヘッダーは、サンプルのヘッダー値か、機密情報（セキュリティトークンやアクセスIDなど）を含める必要のある変数として表示されます。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/global/classes \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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

[プラットフォームトラブルシューティングガイド](troubleshooting.md#errors-and-troubleshooting)は、Experience Platformサービスの使用時に発生する可能性があるエラーのリストを提供します。

個々のプラットフォームサービスのトラブルシューティングガイドについては、[service troubleshooting directory](troubleshooting.md#service-troubleshooting-directory)を参照してください。

必要なヘッダーや要求本文を含む、プラットフォームAPIの特定のエンドポイントについて詳しくは、[プラットフォームAPIガイド](#api-guides)を参照してください。

## プラットフォームAPIガイド{#api-guides}

| APIガイド | 説明 |
| --- | --- |
| [[!DNL Access Control] APIガイド](.././access-control/api/getting-started.md) | [!DNL Access Control] APIエンドポイントは、指定されたサンドボックス内の特定のリソースに対してユーザーが有効になっている現在のポリシーを取得できます。 その他すべてのアクセス制御機能は、[Adobe Admin Console](https://adminconsole.adobe.com/)を通じて提供されます。 |
| [バッチ取り込みAPIガイド](.././ingestion/batch-ingestion/api-overview.md) | Adobe Experience Platform[!DNL Data Ingestion] APIを使用すると、データをバッチファイルとしてプラットフォームに取り込むことができます。 取り込まれるデータは、CRMシステムのフラットファイル（Parketファイルなど）のプロファイルデータ、またはスキーマレジストリ(XDM)の既知のスキーマに準拠するデータのいずれかです。 |
| [[!DNL Catalog Service] APIガイド](.././catalog/api/getting-started.md) | [!DNL Catalog Service] APIを使用すると、開発者はAdobe Experience Platformでデータセットメタデータを管理できます。 これには、データの場所、処理ステージ、処理中に発生したエラーおよびデータレポートが含まれます。 |
| [[!DNL Data Access] APIガイド](.././data-access/api.md) | [!DNL Data Access] APIを使用すると、Experience Platform内で取り込んだデータセットに関する情報を開発者が取得できます。 これには、データセットファイルのアクセスとダウンロード、ヘッダー情報の取得、失敗したバッチと成功したバッチの一覧表示、プレビューCSV / Parketファイルのダウンロードが含まれます。 |
| [[!DNL Dataset Service] APIガイド](.././data-governance/labels/dataset-api.md) | Dataset Service APIを使用すると、データセットの使用ラベルを適用および編集できます。 これはAdobe Experience Platformのデータカタログ機能の一部ですが、データセットメタデータを管理するCatalog Service APIとは別のものです。 |
| [[!DNL Flow Service] APIガイド](.././sources/tutorials/api/create-dataset-base-connection.md) <br> （ソースと宛先） | [!DNL Flow Service] APIは、さまざまな異なるソースからデータを収集および一元化するために使用され、Adobe Experience Platform内の様々な宛先にデータを作成およびアクティブ化するために使用されます。 このサービスはRESTful APIを提供し、サポートされるすべてのソースを接続できます。 |
| [[!DNL Identity Service] APIガイド](.././identity-service/api/getting-started.md) | [!DNL Identity Service] APIを使用すると、開発者は、Adobe Experience PlatformのIDグラフを使用して、デバイス間、チャネル間、およびほぼリアルタイムでの顧客の識別を管理できます。 |
| [[!DNL Observability Insights] APIガイド](.././observability/api/overview.md) | [!DNL Observability Insights] は、開発者がAdobe Experience Platformで主要な監視可能な指標を公開できるRESTful APIです。これらの指標は、Platform の使用状況の統計、Platform サービスのヘルスチェック、様々な Platform 機能の過去の傾向とパフォーマンス指標に関する洞察を提供します。 |
| [[!DNL Policy Service] APIガイド](.././data-governance/api/overview.md) <br> （データガバナンス） | [!DNL Policy Service] APIを使用すると、データ使用ラベルとポリシーを作成および管理し、特定のデータ使用ラベルを含むデータに対して実行できるマーケティングアクションを決定できます。 データセットとフィールドにラベルを適用する方法については、[[!DNL Dataset Service] API](.././data-governance/labels/dataset-api.md)ガイドを参照してください |
| [[!DNL Privacy Service] APIガイド](.././privacy-service/api/getting-started.md) | [!DNL Privacy Service] APIを使用すると、開発者は、プライバシーに関する法的規制に準拠して、Experience Cloudアプリケーション全体で個人データにアクセスしたり削除したりするための顧客リクエストを作成および管理できます。 |
| [[!DNL Query Service] APIガイド](.././query-service/api/getting-started.md) | [!DNL Query Service] APIを使うと、開発者は標準SQLを使ってAdobe Experience Platformデータをクエリできます。 |
| [[!DNL Real-time Customer Profile] APIガイド](.././profile/api/overview.md) | Real-time Customer CustomerプロファイルAPIを使用すると、プロファイルの表示、結合ポリシーの作成と更新、プロファイルデータの書き出しとサンプリング、不要になった、またはエラーになったプロファイルデータの削除など、プロファイルデータの調査と操作が可能です。 |
| [Sandbox APIガイド](.././sandboxes/api/getting-started.md) | Sandbox APIを使用すると、開発者はAdobe Experience Platformの個別の仮想Sandbox環境をプログラムで管理できます。 |
| [[!DNL Schema Registry] APIガイド](.././xdm/api/overview.md) <br> (XDM) | [!DNL Schema Registry] APIを使用すると、開発者は、Adobe Experience Platform内のすべてのスキーマおよび関連するExperience Data Model(XDM)リソースをプログラムで管理できます。 |
| [[!DNL Segmentation Service] APIガイド](.././segmentation/api/overview.md) | [!DNL Segmentation Service] APIを使用すると、開発者はAdobe Experience Platformでセグメント化操作をプログラム的に管理できます。 これには、セグメントの作成や、リアルタイム顧客プロファイルデータからのオーディエンスの生成が含まれます。 |
| [[!DNL Sensei Machine Learning] APIガイド](.././data-science-workspace/api/getting-started.md) <br> (Data Science Workspace) | [!DNL Sensei Machine Learning] APIは、データ科学者がアルゴリズムのオンボーディング、実験、およびサービスの展開から機械学習(ML)サービスを編成および管理するメカニズムを提供します。 |

各サービスで使用できる特定のエンドポイントと操作について詳しくは、Adobe I/Oの[APIリファレンスドキュメント](http://www.adobe.com/go/platform-api-reference-en)を参照してください。

## 次の手順

このドキュメントでは、必要なヘッダー、利用可能なガイド、API呼び出しの例を紹介しました。 Adobe Experience PlatformでAPI呼び出しを行うのに必要なヘッダー値が揃ったので、[プラットフォームAPIガイドテーブル](#api-guides)から、調査するAPIエンドポイントを選択します。

よくある質問に対する回答は、[プラットフォームのトラブルシューティングガイド](troubleshooting.md)を参照してください。

Postman環境を設定し、使用可能なPostmanコレクションを調べるには、『[Platform Postmanガイド](postman.md)』を参照してください。

