---
title: セルフサービスソース（ストリーミング SDK）の概要
description: このドキュメントでは、セルフサービスソース（ストリーミング SDK）を使用して新しいソースを作成する前に知っておく必要がある前提条件の情報について説明します。
hide: true
hidefromtoc: true
source-git-commit: 7744fef9751212a40f8f20df52812d38130c42fc
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 36%

---

# セルフサービスソース（ストリーミング SDK）の概要

セルフサービスソース（ストリーミング SDK）を使用すると、独自のソースを統合して、ストリーミングデータをAdobe Experience Platformに取り込むことができます。 このドキュメントでは、 [[!DNL Flow Service] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml).

## プロセスの概要

Experience Platformでソースを設定する手順を以下に示します。

### 統合

* [ストリーミング SDK 用の新しい接続仕様の作成](create.md).
* [新しい接続仕様 ID でストリーミングフロー仕様を更新する](update-flow-specs.md).
* [ストリーミングソースをテストして送信する](submit.md).

### ドキュメント

* ソースのドキュメント化を開始するには、 [セルフサービスソースのドキュメント作成の概要](../documentation/doc-overview.md).
* 次のガイドを読む： [GitHub Web インターフェイスの使用](../documentation/github.md) を参照してください。
* 次のガイドを読む： [テキストエディターの使用](../documentation/text-editor.md) ローカルマシンを使用してドキュメントを作成する手順については、を参照してください。
* [ストリーミング SDK API ドキュメントテンプレートを使用して、API でソースをドキュメント化する](streaming-template-api.md).
* [ストリーミング SDK UI ドキュメントテンプレートを使用して、UI でソースをドキュメント化する](streaming-template-ui.md).

また、以下のドキュメントテンプレートをダウンロードすることもできます。

* [API ドキュメントのテンプレート](../assets/streaming/streaming-template-api.zip)
* [UI ドキュメントテンプレート](../assets/streaming/streaming-template-ui.zip)

## 前提条件

>[!IMPORTANT]
>
>Experience Platformと統合するソースは、更新を送信するために、エンドポイントが購読できる Webhook をサポートできる必要があります。

セルフサービスソース（ストリーミング SDK）を使用するには、Adobe Experience Platform Sources でプロビジョニングされたサンドボックス組織に対するアクセス権があることを確認する必要があります。

また、このガイドでは、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## API 呼び出し例の読み取り

セルフサービスソース（ストリーミング SDK）および [!DNL Flow Service] API ドキュメントには、API 呼び出しの例が記載されており、リクエストの形式を設定する方法を示しています。 これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

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

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 次の手順

セルフサービスソース（ストリーミング SDK）を使用して新しいソースの作成を開始するには、 [新しいソースの作成](./create.md).
