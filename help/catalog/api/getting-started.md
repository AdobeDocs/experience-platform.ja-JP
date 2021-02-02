---
keywords: Experience Platform；ホーム；人気のあるトピック；カタログサービス；カタログ；カタログサービス；カタログサービス；カタログ
solution: Experience Platform
title: カタログサービス開発者ガイド
topic: developer guide
description: この開発者ガイドでは、カタログ API の使用を始める手順を説明します。次に、カタログを使用して主要な操作を実行するためのサンプル API 呼び出しを提供します。
translation-type: tm+mt
source-git-commit: ece2ae1eea8426813a95c18096c1b428acfd1a71
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 44%

---


# [!DNL Catalog Service] 開発ガイド

[!DNL Catalog Service] は、Adobe Experience Platform内のデータの位置と系統に関する記録システムです。[!DNL Catalog] は、データ自体にアクセスすることなく、データに関する情報を検索できるメタデータストア（「カタログ」） [!DNL Experience Platform]として機能します。詳しくは、[[!DNL Catalog] 概要](../home.md)を参照してください。

この開発者ガイドでは、[!DNL Catalog] APIを使用して開始を行う際に役立つ手順を説明します。 次に、[!DNL Catalog]を使用して主要な操作を実行するためのサンプルAPI呼び出しを提供します。

## 前提条件

[!DNL Catalog] では、様々な種類のリソースおよび操作のメタデータを追跡 [!DNL Experience Platform]します。この開発者ガイドでは、以下のリソースの作成と管理に関連する様々な[!DNL Experience Platform]サービスについて、十分に理解している必要があります。

* [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):顧客体験データを [!DNL Platform] 編成する際に使用される標準化されたフレームワーク。
* [バッチ取得](../../ingestion/batch-ingestion/overview.md)[!DNL Experience Platform]： が CSV や Parket などのデータファイルからデータを取得して保存する方法。
* [ストリーミング取得](../../ingestion/streaming-ingestion/overview.md)[!DNL Experience Platform]： がクライアントサイドおよびサーバーサイドのデバイスからデータをリアルタイムで取得し、保存する方法。

以下の節では、[!DNL Catalog Service] APIを正しく呼び出すために知る必要がある、または手元に置く必要がある追加情報について説明します。

## API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

## 必須ヘッダーの値の収集

[!DNL Platform] APIを呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。 次に示すように、すべての[!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソースは、特定の仮想サンドボックスに分離されています。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform]のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

* Content-Type: application/json

## [!DNL Catalog] API呼び出しのベストプラクティス

[!DNL Catalog] APIに対してGETリクエストを実行する場合、必要なオブジェクトとプロパティのみを返すために、リクエストにクエリパラメーターを含めることをお勧めします。 フィルターを適用しないリクエストの応答ペイロードのサイズは 3 GB に達っすることがあり、全体的なパフォーマンスが低下する可能性があります。

特定のオブジェクトを表示するには、リクエストパスに ID を含めるか、または `properties` や `limit` などのクエリーパラメーターを使用して応答をフィルターします。フィルターは、ヘッダーおよびクエリーパラメーターとして渡すことができ、クエリーパラメーターとして渡されたフィルターが優先されます。詳しくは、[カタログデータのフィルター](filter-data.md)に関するドキュメントを参照してください。

一部のクエリではAPIに大きな負荷がかかる場合があるので、ベストプラクティスをさらにサポートするために、[!DNL Catalog]クエリにグローバル制限が導入されています。

## 次の手順

このドキュメントでは、[!DNL Catalog] APIを呼び出すために必要な前提条件に関する知識を説明しました。 次に、この開発者ガイドに記載されているサンプル呼び出しに進み、その手順に従うことができます。

このガイドの例のほとんどは`/dataSets`エンドポイントを使用していますが、この原則は[!DNL Catalog]内の他のエンドポイント（`/batches`や`/accounts`など）に適用できます。 各エンドポイントで使用できるすべての呼び出しと操作の完全なリストについては、『[カタログサービス API リファレンス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)』を参照してください。

[!DNL Catalog] APIとデータ取り込みとの関わり方を示すワークフローについては、[データセット](../datasets/create.md)の作成に関するチュートリアルを参照してください。
