---
keywords: エクスペリエンス Platform、home、人気のある話題。Adobe エクスペリエンス Platform、api ガイド、プラットフォーム api ガイド、プラットフォーム入門、デベロッパーズガイド
solution: Experience Platform
title: Adobe エクスペリエンスプラットフォーム Api の概要
topic-legacy: api guide
description: Adobe エクスペリエンスプラットフォームは、相互に緊密にリンクされた API サービスを提供します。 このガイドには、使用可能なサービス、CRUD 操作に必要なヘッダー、エラーメッセージ、Postman コレクション、およびサンプル API 呼び出しについての情報が記載されています。
exl-id: a362bcb4-a908-43a8-abd3-0e1d21cb9117
source-git-commit: e62e4e3a12ad2a85de5b10c60fde3618cde84c4b
workflow-type: tm+mt
source-wordcount: '1379'
ht-degree: 30%

---

# Adobe エクスペリエンスプラットフォーム Api の概要

Adobe エクスペリエンスプラットフォームは、「API first」理念に基づいて開発されています。 プラットフォーム Api を使用して、計算属性の設定、データまたはエンティティへのアクセス、データのエクスポート、不要なデータまたはバッチの削除など、プログラムによってデータに対して基本的な CRUD (作成、読み取り、更新、削除) 操作を実行できます。

各エクスペリエンスプラットフォームサービスの Api は、同じ認証ヘッダーのセットを共有し、CRUD 操作に同様の構文を使用します。 次のガイドでは、Platform Api を使用して作業を開始するために必要な手順を示します。

## 認証とヘッダー

プラットフォームのエンドポイントを正常に呼び出すには、認証チュートリアルを完了しておく必要があり [ ](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis) ます。 次に示すように、Experience Platform API 呼び出しの必要な各ヘッダーの値は、認証に関するチュートリアルで説明されています。

- `Authorization: Bearer {ACCESS_TOKEN}`
- `x-api-key: {API_KEY}`
- `x-gw-ims-org-id: {IMS_ORG}`

### サンドボックスヘッダー

Experience Platform のすべてのリソースは、特定の仮想サンドボックスに分離されています。Platform API へのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- `x-sandbox-name: {SANDBOX_NAME}`

Platform のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../sandboxes/home.md)を参照してください。

### Content-type ヘッダー

リクエスト本文にペイロードを持つすべてのリクエスト（POST、PUT、PATCH 呼び出しなど）には、`Content-Type` ヘッダーが含まれている必要があります。受け入れられる値は、API エンドポイントによって異なります。 `Content-Type`エンドポイントに特定の値が必要な場合、その値は、 [ 個々のプラットフォームサービスの api ガイドによって提供されるサンプル api 要求に表示され ](#api-guides) ます。

## Experience Platform API の基本

Adobe エクスペリエンスプラットフォーム Api には、プラットフォームリソースを効果的に管理するために重要な、理解を深めるための基礎となるテクノロジと構文がいくつか含まれています。

「JSON スキーマオブジェクトの例」など、基盤となる API テクノロジプラットフォームの利用について詳しくは、 [ 『エクスペリエンス PLATFORM API の基礎ガイド』を参照してください ](api-fundamentals.md) 。

## Postman コレクション (経験 Platform Api 用)

Postman は、事前に定義された変数を使用して環境を設定したり、API コレクションを共有したり、CRUD 要求を効率的に実行したりできるようにする、API 開発用コラボレーションプラットフォームです。 ほとんどのプラットフォーム API サービスには Postman コレクションが含まれています。これを使用すると、API 呼び出しの実行に役立てることができます。

Postman について詳しくは、環境の設定方法、使用可能なコレクションのリスト、およびコレクションのインポート方法について詳しくは、Platform Postman のマニュアルを参照してください [ ](postman.md) 。

## API 呼び出し例の読み取り {#sample-api}

リクエストの形式は、使用する Platform API によって異なります。API 呼び出しの構造を学ぶ最善の方法は、使用している特定の Platform サービスのドキュメントに記載されている例に従うことです。

[!DNL Experience Platform]API 呼び出し例については、2つの異なる方法で説明されています。まず、呼び出しは **API 形式**&#x200B;で表されます。これは、操作（GET、POST、PUT、PATCH、DELETE など）と使用中のエンドポイント（例えば、`/global/classes`）のみを示すテンプレート表現です。また、テンプレートには、`GET /{VARIABLE}/classes/{ANOTHER_VARIABLE}` などの呼び出しの作成方法を示すために、変数の位置を示すものもあります。

その後、呼び出しは、**リクエスト**&#x200B;内の cURL コマンドとして表示されます。これには、API とのやり取りに必要なヘッダーと完全な「ベースパス」が含まれます。ベースパスは、すべてのエンドポイントの前に追加する必要があります。例えば、前述の `/global/classes` エンドポイントは `https://platform.adobe.io/data/foundation/schemaregistry/global/classes` になります。詳しくは、ドキュメント全体にわたって API フォーマット/要求パターンが表示されます。また、独自のプラットフォーム Api を呼び出す場合は、例の要求に示されている完全なパスを使用する必要があります。

### API リクエストの例

以下は、ドキュメントで使用される形式を示す API リクエストの例です。

**API 形式**

API 形式は、操作（GET）と使用されているエンドポイントを示します。変数は中括弧で示されます（この場合は `{CONTAINER_ID}`）。

```http
GET /{CONTAINER_ID}/classes
```

**リクエスト**

この例のリクエストでは、API 形式の変数には、リクエストパス内の実際の値が与えられます。さらに、すべての必要なヘッダーは、機密性の高い情報 (セキュリティトークンやアクセス Id など) を含める必要がある、サンプルのヘッダー値または変数として表示されます。

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

[プラットフォームのトラブルシューティングガイドには、 ](troubleshooting.md#errors-and-troubleshooting) エクスペリエンスプラットフォームサービスを使用している場合に発生する可能性があるエラーの一覧が記載されています。

個々のプラットフォームサービスについて詳しくは、 [ サービスのトラブルシューティングディレクトリを参照してください ](troubleshooting.md#service-troubleshooting-directory) 。

必要なヘッダーと要求本文を含む、プラットフォーム Api の特定のエンドポイントについて詳しくは、 [ PLATFORM api ガイドを参照してください ](#api-guides) 。

## プラットフォーム API ガイド {#api-guides}

| API ガイド | 説明 |
| --- | --- |
| [[!DNL Access Control] API ガイド](.././access-control/api/getting-started.md) | [!DNL Access Control]API エンドポイントは、指定されたサンドボックス内の特定のリソースについて、ユーザーに有効なポリシーを取得することができます。その他のアクセス制御機能はすべて、アドビシステムズ社の管理コンソールから提供されてい [ ](https://adminconsole.adobe.com/) ます。 |
| [Batch インジェスト API ガイド](.././ingestion/batch-ingestion/api-overview.md) | Adobe エクスペリエンスプラットフォーム [!DNL Data Ingestion] API を使用すると、データをバッチファイルとしてプラットフォームに取り込むことができます。 Ingested のデータとして、CRM システムのフラットファイル (Parquet ファイルなど)、またはスキーマレジストリの既知のスキーマに適合するデータ (XDM) のプロファイルデータを指定することができます。 |
| [[!DNL Catalog Service] API ガイド](.././catalog/api/getting-started.md) | この [!DNL Catalog Service] API を使用すると、開発者は、Adobe エクスペリエンスプラットフォームでデータセットメタデータを管理できます。 これには、データの場所、処理段階、処理中に発生したエラー、およびデータレポートが含まれます。 |
| [[!DNL Data Access] API ガイド](.././data-access/api.md) | この [!DNL Data Access] API を使用すると、開発者はエクスペリエンスプラットフォーム内の ingested データセットに関する情報を取得できます。 これには、データセットファイルのアクセスとダウンロード、ヘッダー情報の取得、失敗したバッチと成功したバッチの一覧表示、CSV/Parquet ファイルのプレビューが含まれます。 |
| [[!DNL Dataset Service] API ガイド](.././data-governance/labels/dataset-api.md) | Dataset Service API を使用すると、データセットの使用ラベルを適用および編集できます。これは Adobe Experience Platform のデータカタログ機能の一部ですが、データセットメタデータを管理する Catalog Service API とは別のものです。 |
| [[!DNL Identity Service] API ガイド](.././identity-service/api/getting-started.md) | この [!DNL Identity Service] API を使用すると、開発者は、Adobe エクスペリエンスプラットフォームの id グラフを使用して、デバイス間、クロスチャネル、およびほぼリアルタイムでの顧客の id を管理することができます。 |
| [[!DNL Observability Insights] API ガイド](.././observability/api/overview.md) | [!DNL Observability Insights] は、開発者が Adobe エクスペリエンスプラットフォームの重要な observability メトリックスを公開できる RESTful API です。 これらの指標は、Platform の使用状況の統計、Platform サービスのヘルスチェック、様々な Platform 機能の過去の傾向とパフォーマンス指標に関する洞察を提供します。 |
| [[!DNL Policy Service] API ガイド ](.././data-governance/api/overview.md) <br> (データガバナンス) | この API を使用すると、データ [!DNL Policy Service] 使用のラベルとポリシーを作成および管理して、特定のデータ使用ラベルを含むデータに対してどのマーケティングアクションを実行できるかを判断できます。 データセットとフィールドにラベルを適用するには、「 [[!DNL Dataset Service]  API ガイド」を参照してください。 ](.././data-governance/labels/dataset-api.md) |
| [[!DNL Privacy Service] API ガイド](.././privacy-service/api/getting-started.md) | この [!DNL Privacy Service] API により、開発者は、法律上のクラウドアプリケーション間で、法的プライバシーに関する規制に準拠した状態で、個人データにアクセスしたり削除したりすることができます。 |
| [[!DNL Query Service] API ガイド](.././query-service/api/getting-started.md) | この [!DNL Query Service] API を使用すると、開発者は標準 SQL を使用して Adobe エクスペリエンスプラットフォームデータをクエリーできます。 |
| [[!DNL Real-time Customer Profile] API ガイド](.././profile/api/overview.md) | リアルタイムカスタマープロファイル API を使用すると、開発者は、プロファイルの表示、マージポリシーの作成と更新、プロファイルデータの書き出しまたはサンプリング、不要になったプロファイルデータやエラーによって追加されたプロファイルデータの削除など、プロファイルデータを確認して操作することができます。 |
| [サンドボックス API ガイド](.././sandboxes/api/getting-started.md) | サンドボックス API を使用すると、開発者は、Adobe エクスペリエンスプラットフォームを使用して、隔離された仮想サンドボックス環境をプログラムで管理できます。 |
| [[!DNL Schema Registry] API ガイド ](.././xdm/api/overview.md) <br> (XDM) | この [!DNL Schema Registry] API を使用すると、開発者は、Adobe エクスペリエンスプラットフォーム内のすべてのスキーマおよび関連する経験データモデル (XDM) リソースをプログラムで管理できます。 |
| [[!DNL Segmentation Service] API ガイド](.././segmentation/api/overview.md) | この [!DNL Segmentation Service] API を使用すると、開発者は、Adobe エクスペリエンスプラットフォームでプログラムによってセグメンテーション操作を管理することができます。 これには、作成セグメントが含まれています。また、リアルタイムの顧客プロファイルデータから対象ユーザーが生成されます。 |
| [[!DNL Sensei Machine Learning] API ガイド ](.././data-science-workspace/api/getting-started.md) <br> (Data サイエンス Workspace) | この API は、データ科学者が、 [!DNL Sensei Machine Learning] アルゴリズムのオンボード、実験、サービスのデプロイから machine learning (ML) サービスを編成および管理するためのメカニズムを提供します。 |

各サービスで使用可能な特定のエンドポイントと操作について詳しくは、『 Adobe i/o についての API リファレンス』を参照してください [ ](https://www.adobe.com/go/platform-api-reference-en) 。

## 次の手順

このドキュメントでは、必要なヘッダー、使用可能なガイドを紹介し、API 呼び出しについて説明しました。 これで、Adobe エクスペリエンスプラットフォームで API を呼び出すために必要なヘッダー値を取得できたので、プラットフォーム API ガイドテーブルから、検証する API エンドポイントを選択し [ ](#api-guides) ます。

よくある質問への回答については、「 [ プラットフォームのトラブルシューティングガイド」を参照してください ](troubleshooting.md) 。

Postman 環境を設定して、使用可能な Postman コレクションを調べるには、『 [ Platform Postman guide 』を参照してください ](postman.md) 。
