---
keywords: Experience Platform；ホーム；人気のトピック；catalog service；カタログ；Catalog service；カタログ
solution: Experience Platform
title: Catalog Service API ガイド
description: Catalog Service API を使用すると、開発者はAdobe Experience Platformでデータセットメタデータを管理できます。 このガイドに従って、API を使用した主な操作の実行方法を学習します。
exl-id: 812fcdae-ed0e-4f2b-84d7-26f2f79e71b9
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 58%

---

# [!DNL Catalog Service] API ガイド

[!DNL Catalog Service] は、Adobe Experience Platform 内のデータの場所と系列の記録システムです。[!DNL Catalog] はメタデータストア（「カタログ」）として機能し、データ自体にアクセスしなくても、[!DNL Experience Platform] 内のデータに関する情報を検索できます。 詳しくは、[[!DNL Catalog] 概要](../home.md)を参照してください。

このデベロッパーガイドでは、[!DNL Catalog] API を使い始めるのに役立つ手順を説明します。次に、このガイドでは、[!DNL Catalog] を使用して主要な操作を実行するための API 呼び出しのサンプルを提供します。

## 前提条件

[!DNL Catalog] は、[!DNL Experience Platform] 内の複数の種類のリソースおよび操作のメタデータを追跡します。 このデベロッパーガイドでは、これらのリソースの作成と管理に関わる様々な [!DNL Experience Platform] サービスについて、実際に理解している必要があります。

* [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：[!DNL Experience Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。
* [&#x200B; バッチ取り込み &#x200B;](../../ingestion/batch-ingestion/overview.md):[!DNL Experience Platform] が CSV や Parquet などのデータファイルからデータを取り込んで保存する方法。
* [&#x200B; ストリーミング取得 &#x200B;](../../ingestion/streaming-ingestion/overview.md):[!DNL Experience Platform] がクライアントサイドおよびサーバーサイドのデバイスから、リアルタイムにデータを取得して保存する方法。

次の節では、[!DNL Catalog Service] API の呼び出しを正しくおこなうために知っておく必要がある、または手元に用意しておく必要がある追加情報を提供します。

## API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

## 必須ヘッダーの値の収集

[!DNL Experience Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* Authorization： Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Experience Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Experience Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次のような追加ヘッダーが必要です。

* Content-Type：application/json

## [!DNL Catalog] API 呼び出しのベストプラクティス

[!DNL Catalog] API に対してGET リクエストを実行する場合、必要なオブジェクトとプロパティのみを返すために、リクエストにクエリパラメーターを含めることをお勧めします。 フィルターを適用しないリクエストの応答ペイロードのサイズは 3 GB に達っすることがあり、全体的なパフォーマンスが低下する可能性があります。

特定のオブジェクトを表示するには、リクエストパスに ID を含めるか、または `properties` や `limit` などのクエリーパラメーターを使用して応答をフィルターします。フィルターは、ヘッダーおよびクエリーパラメーターとして渡すことができ、クエリーパラメーターとして渡されたフィルターが優先されます。詳しくは、[カタログデータのフィルター](filter-data.md)に関するドキュメントを参照してください。

一部のクエリは API に大きな負荷がかかる可能性があるので、ベストプラクティスをさらにサポートするために、[!DNL Catalog] クエリに対してグローバルな制限が実装されました。

## 次の手順

このドキュメントでは、[!DNL Catalog] API を呼び出すために必要な前提条件に関する知識を説明しました。これで、この開発者ガイドに記載されているサンプル呼び出しに進んで、その手順に従うことができます。

このガイドの例のほとんどは `/dataSets` エンドポイントを使用しますが、原則は [!DNL Catalog] 内の他のエンドポイント（`/batches` など）にも適用できます。 各エンドポイントで使用できるすべての呼び出しと操作の完全なリストについては、『[カタログサービス API リファレンス](https://www.adobe.io/experience-platform-apis/references/catalog/)』を参照してください。

[!DNL Catalog] API がデータ取り込みにどのように関係するかを示す詳細なワークフローについては、[&#x200B; データセットの作成 &#x200B;](../datasets/create.md) に関するチュートリアルを参照してください。
