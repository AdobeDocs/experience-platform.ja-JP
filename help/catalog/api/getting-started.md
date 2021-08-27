---
keywords: Experience Platform；ホーム；人気のあるトピック；カタログサービス；カタログ；カタログサービス；カタログ
solution: Experience Platform
title: カタログサービスAPIガイド
topic-legacy: developer guide
description: カタログサービスAPIを使用すると、開発者はAdobe Experience Platformでデータセットメタデータを管理できます。 このガイドに従って、API を使用した主な操作の実行方法を学習します。
exl-id: 812fcdae-ed0e-4f2b-84d7-26f2f79e71b9
source-git-commit: 5160bc8057a7f71e6b0f7f2d594ba414bae9d8f6
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 57%

---

# [!DNL Catalog Service] API ガイド

[!DNL Catalog Service] は、Adobe Experience Platform 内のデータの場所と系列の記録システムです。[!DNL Catalog] は、内のデータに関する情報を検索できるメタデータストア（「カタログ」）として機能しま [!DNL Experience Platform]す。データ自体にアクセスする必要はありません。詳しくは、[[!DNL Catalog] 概要](../home.md)を参照してください。

このデベロッパーガイドでは、[!DNL Catalog] API を使い始めるのに役立つ手順を説明します。次に、[!DNL Catalog]を使用して主要な操作を実行するAPI呼び出しの例を示します。

## 前提条件

[!DNL Catalog] では、様々な種類のリソースおよび操作のメタデータを追跡しま [!DNL Experience Platform]す。この開発者ガイドでは、以下のリソースの作成と管理に関する様々な[!DNL Experience Platform]サービスに関する十分な知識が必要です。

* [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Platform] に使用される標準化されたフレームワーク。
* [バッチ取得](../../ingestion/batch-ingestion/overview.md)[!DNL Experience Platform]： が CSV や Parket などのデータファイルからデータを取得して保存する方法。
* [ストリーミング取得](../../ingestion/streaming-ingestion/overview.md):クライ [!DNL Experience Platform] アントサイドおよびサーバーサイドのデバイスからデータをリアルタイムで取得して保存する方法。

以下の節では、[!DNL Catalog Service] APIを正しく呼び出すために知っておく必要がある、または手元に置く必要がある追加情報を示します。

## API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

## 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* Authorization： Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id： `{IMS_ORG}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name： `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

* Content-Type： application/json

## [!DNL Catalog] API呼び出しのベストプラクティス

[!DNL Catalog] APIに対してGETリクエストを実行する場合、必要なオブジェクトとプロパティのみを返すように、リクエストにクエリパラメーターを含めることをお勧めします。 フィルターを適用しないリクエストの応答ペイロードのサイズは 3 GB に達っすることがあり、全体的なパフォーマンスが低下する可能性があります。

特定のオブジェクトを表示するには、リクエストパスに ID を含めるか、または `properties` や `limit` などのクエリーパラメーターを使用して応答をフィルターします。フィルターは、ヘッダーおよびクエリーパラメーターとして渡すことができ、クエリーパラメーターとして渡されたフィルターが優先されます。詳しくは、[カタログデータのフィルター](filter-data.md)に関するドキュメントを参照してください。

一部のクエリはAPIに大きな負荷をかける可能性があるので、ベストプラクティスをさらにサポートするために、[!DNL Catalog]クエリにグローバル制限が実装されました。

## 次の手順

このドキュメントでは、[!DNL Catalog] APIを呼び出すために必要な前提条件に関する知識を説明しました。 これで、この開発者ガイドに記載されているサンプル呼び出しに進んで、その手順に従うことができます。

このガイドの例のほとんどは`/dataSets`エンドポイントを使用しますが、原則は[!DNL Catalog]内の他のエンドポイント（`/batches`や`/accounts`など）に適用できます。 各エンドポイントで使用できるすべての呼び出しと操作の完全なリストについては、『[カタログサービス API リファレンス](https://www.adobe.io/experience-platform-apis/references/catalog/)』を参照してください。

[!DNL Catalog] APIとデータ取得との関わり方を示すワークフローについては、[データセット](../datasets/create.md)の作成に関するチュートリアルを参照してください。
