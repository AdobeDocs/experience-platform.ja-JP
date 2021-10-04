---
keywords: Experience Platform；ホーム；人気のあるトピック；Adobe Experience Platform;api ガイド；プラットフォーム api ガイド；プラットフォームの概要；開発者ガイド
solution: Experience Platform
title: Adobe Experience Platform API の概要
topic-legacy: api guide
description: Adobe Experience Platformは、互いに密接に関連する API サービスを提供します。 このガイドには、使用可能なサービス、CRUD 操作に必要なヘッダー、エラーメッセージ、Postman コレクション、サンプル API 呼び出しに関する情報が含まれています。
exl-id: a362bcb4-a908-43a8-abd3-0e1d21cb9117
source-git-commit: a0f4e49192a54075ce7c48620c9729e61ecdfdac
workflow-type: tm+mt
source-wordcount: '1425'
ht-degree: 29%

---

# Adobe Experience Platform API の概要

Adobe Experience Platformは、「API ファースト」の理念の下で開発されています。 Platform API を使用すると、計算済み属性の設定、データ/エンティティへのアクセス、データのエクスポート、不要なデータやバッチの削除など、データに対する基本的な CRUD（作成、読み取り、更新、削除）操作をプログラムで実行できます。

各認証サービスの API はすべて同じExperience Platformヘッダーのセットを共有し、CRUD 操作に同様の構文を使用します。 次のガイドでは、Platform API の使用を開始するために必要な手順の概要を説明します。

## 認証とヘッダー

Platform エンドポイントを正しく呼び出すには、[ 認証に関するチュートリアル ](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis) を完了する必要があります。 次に示すように、Experience Platform API 呼び出しの必要な各ヘッダーの値は、認証に関するチュートリアルで説明されています。

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {IMS_ORG}`

### サンドボックスヘッダー

Experience Platform のすべてのリソースは、特定の仮想サンドボックスに分離されています。Platform API へのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- `x-sandbox-name: {SANDBOX_NAME}`

Platform のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../sandboxes/home.md)を参照してください。

### Content-type ヘッダー

リクエスト本文にペイロードを持つすべてのリクエスト（POST、PUT、PATCH 呼び出しなど）には、`Content-Type` ヘッダーが含まれている必要があります。指定できる値は、各 API エンドポイントに固有です。 特定の `Content-Type` 値がエンドポイントに必要な場合、その値は、個々の Platform サービス ](#api-guides) の [API ガイドによって提供される API リクエストの例に示されます。

## Experience Platform API の基本

Adobe Experience Platform API は、Platform リソースを効果的に管理するために理解する必要がある、基盤となる複数のテクノロジーと構文を使用しています。

JSON スキーマオブジェクトの例など、Platform が利用する基になる API テクノロジーの詳細については、『[Experience PlatformAPI の基本事項 ](api-fundamentals.md)』ガイドを参照してください。

## Experience PlatformAPI 用の Postman コレクション

Postman は、API 開発のコラボレーションプラットフォームで、事前に設定された変数を使用して環境を設定し、API コレクションを共有し、CRUD リクエストを合理化するなどを可能にします。 ほとんどの Platform API サービスには、API 呼び出しの実行に役立つ Postman コレクションがあります。

環境の設定方法、利用可能なコレクションのリスト、コレクションの読み込み方法など、Postman について詳しくは、[Platform Postman のドキュメント ](postman.md) を参照してください。

## API 呼び出し例の読み取り {#sample-api}

リクエストの形式は、使用する Platform API によって異なります。API 呼び出しの構造を学ぶ最善の方法は、使用している特定の Platform サービスのドキュメントに記載されている例に従うことです。

[!DNL Experience Platform] のドキュメントでは、API 呼び出しの例が 2 つの異なる方法で示されています。 まず、呼び出しは **API 形式**&#x200B;で表されます。これは、操作（GET、POST、PUT、PATCH、DELETE など）と使用中のエンドポイント（例えば、`/global/classes`）のみを示すテンプレート表現です。また、テンプレートには、`GET /{VARIABLE}/classes/{ANOTHER_VARIABLE}` などの呼び出しの作成方法を示すために、変数の位置を示すものもあります。

その後、呼び出しは、**リクエスト**&#x200B;内の cURL コマンドとして表示されます。これには、API とのやり取りに必要なヘッダーと完全な「ベースパス」が含まれます。ベースパスは、すべてのエンドポイントの前に追加する必要があります。例えば、前述の `/global/classes` エンドポイントは `https://platform.adobe.io/data/foundation/schemaregistry/global/classes` になります。API 形式/リクエストパターンはドキュメント全体に表示され、独自に Platform API を呼び出す場合は、この例のリクエストに示す完全なパスを使用する必要があります。

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

[Platform トラブルシューティングガイド ](troubleshooting.md#errors-and-troubleshooting) に、Experience Platformサービスの使用時に発生する可能性のあるエラーのリストを示します。

個々の Platform サービスのトラブルシューティングガイドについては、[ サービストラブルシューティングディレクトリ ](troubleshooting.md#service-troubleshooting-directory) を参照してください。

必要なヘッダーやリクエスト本文を含む、Platform API の特定のエンドポイントについて詳しくは、[Platform API ガイド ](#api-guides) を参照してください。

## Platform API ガイド {#api-guides}

| API ガイド | 説明 |
| --- | --- |
| [[!DNL Access Control] API ガイド](.././access-control/api/getting-started.md) | [!DNL Access Control] API エンドポイントは、指定したサンドボックス内の特定のリソースに対してユーザーが有効な現在のポリシーを取得できます。 その他すべてのアクセス制御機能は、[Adobe Admin Console](https://adminconsole.adobe.com/) を通じて提供されます。 |
| [バッチ取得 API ガイド](.././ingestion/batch-ingestion/api-overview.md) | Adobe Experience Platform [!DNL Data Ingestion] API を使用すると、データをバッチファイルとして Platform に取り込むことができます。 CRM システムのフラットファイルのプロファイルデータ（Parquet ファイルなど）、またはスキーマレジストリ (XDM) の既知のスキーマに適合するデータを取り込むことができます。 |
| [[!DNL Catalog Service] API ガイド](.././catalog/api/getting-started.md) | [!DNL Catalog Service] API を使用すると、開発者はAdobe Experience Platformでデータセットメタデータを管理できます。 これには、データの場所、処理ステージ、処理中に発生したエラー、データレポートが含まれます。 |
| [[!DNL Data Access] API ガイド](.././data-access/api.md) | [!DNL Data Access] API を使用すると、開発者は、取り込んだデータセットに関する情報をExperience Platform内で取得できます。 これには、データセットファイルのアクセスとダウンロード、ヘッダー情報の取得、失敗および成功したバッチの一覧表示、CSV のプレビュー/Parquet ファイルのダウンロードが含まれます。 |
| [[!DNL Dataset Service] API ガイド](.././data-governance/labels/dataset-api.md) | Dataset Service API を使用すると、データセットの使用ラベルを適用および編集できます。これは Adobe Experience Platform のデータカタログ機能の一部ですが、データセットメタデータを管理する Catalog Service API とは別のものです。 |
| [[!DNL Flow Service] API ガイド](.././sources/tutorials/api/create-dataset-base-connection.md) <br> （ソースと宛先） | [!DNL Flow Service] API は、様々な異なるソースからデータを収集して一元化するために使用され、Adobe Experience Platform内の様々な宛先に対してデータを作成およびアクティブ化するために使用されます。 このサービスは、RESTful API を提供し、サポートされているすべてのソースから接続できます。 |
| [[!DNL Identity Service] API ガイド](.././identity-service/api/getting-started.md) | [!DNL Identity Service] API を使用すると、開発者は、Adobe Experience Platformの ID グラフを使用して、デバイス間、チャネル間、およびほぼリアルタイムでの顧客の識別を管理できます。 |
| [[!DNL Observability Insights] API ガイド](.././observability/api/overview.md) | [!DNL Observability Insights] は、開発者がAdobe Experience Platformで主要な観察性指標を公開できる RESTful API です。これらの指標は、Platform の使用状況の統計、Platform サービスのヘルスチェック、様々な Platform 機能の過去の傾向とパフォーマンス指標に関する洞察を提供します。 |
| [[!DNL Policy Service] API ガイド](.././data-governance/api/overview.md) <br> （データガバナンス） | [!DNL Policy Service] API を使用すると、データ使用ラベルとポリシーを作成および管理して、特定のデータ使用ラベルを含むデータに対して実行できるマーケティングアクションを決定できます。 データセットとフィールドにラベルを適用するには、[[!DNL Dataset Service] API](.././data-governance/labels/dataset-api.md) ガイドを参照してください。 |
| [[!DNL Privacy Service] API ガイド](.././privacy-service/api/getting-started.md) | [!DNL Privacy Service] API を使用すると、開発者は、プライバシーに関する法規制に準拠して、Experience Cloudアプリケーション全体で自分の個人データにアクセスしたり削除したりするための顧客リクエストを作成および管理できます。 |
| [[!DNL Query Service] API ガイド](.././query-service/api/getting-started.md) | [!DNL Query Service] API を使用すると、開発者は標準の SQL を使用してAdobe Experience Platformデータに対してクエリを実行できます。 |
| [[!DNL Real-time Customer Profile] API ガイド](.././profile/api/overview.md) | リアルタイム顧客プロファイル API を使用すると、プロファイルの表示、結合ポリシーの作成と更新、プロファイルデータの書き出しとサンプリング、不要になった、またはエラーで追加されたプロファイルデータの削除など、プロファイルデータの調査と操作ができます。 |
| [サンドボックス API ガイド](.././sandboxes/api/getting-started.md) | Sandbox API を使用すると、開発者はAdobe Experience Platformで独立した仮想サンドボックス環境をプログラムで管理できます。 |
| [[!DNL Schema Registry] API ガイド](.././xdm/api/overview.md) <br> (XDM) | [!DNL Schema Registry] API を使用すると、開発者はAdobe Experience Platform内のすべてのスキーマと関連する Experience Data Model(XDM) リソースをプログラムで管理できます。 |
| [[!DNL Segmentation Service] API ガイド](.././segmentation/api/overview.md) | [!DNL Segmentation Service] API を使用すると、開発者はAdobe Experience Platformでセグメント化操作をプログラムで管理できます。 これには、セグメントの作成や、リアルタイム顧客プロファイルデータからのオーディエンスの生成が含まれます。 |
| [[!DNL Sensei Machine Learning] API ガイド](.././data-science-workspace/api/getting-started.md) <br> (Data Science Workspace) | [!DNL Sensei Machine Learning] API は、アルゴリズムのオンボーディング、実験、サービスの導入に至るまで、データサイエンティストが機械学習 (ML) サービスを整理および管理するメカニズムを提供します。 |

各サービスで使用できる特定のエンドポイントと操作について詳しくは、Adobe I/Oに関する [API リファレンスドキュメント ](https://www.adobe.com/go/platform-api-reference-en) を参照してください。

## 次の手順

このドキュメントでは、必要なヘッダー、利用可能なガイド、API 呼び出し例の紹介を行いました。 Adobe Experience Platformで API 呼び出しをおこなうために必要な必須のヘッダー値が揃ったので、[Platform API ガイドの表 ](#api-guides) から調査する API エンドポイントを選択します。

よくある質問に対する回答は、[Platform トラブルシューティングガイド ](troubleshooting.md) を参照してください。

Postman 環境を設定し、利用可能な Postman コレクションを調べるには、『[Platform Postman ガイド ](postman.md)』を参照してください。
