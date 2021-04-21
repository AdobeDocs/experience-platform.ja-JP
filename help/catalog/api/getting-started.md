---
keywords: Experience Platform；ホーム；人気のあるトピック；カタログサービス；カタログ；カタログサービス；カタログサービス；カタログ
solution: Experience Platform
title: カタログサービスAPIガイド
topic-legacy: developer guide
description: Catalog Service APIを使用すると、開発者はAdobe Experience Platformでデータセットメタデータを管理できます。 このガイドに従って、APIを使用した主な操作の実行方法を学習します。
exl-id: 812fcdae-ed0e-4f2b-84d7-26f2f79e71b9
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 46%

---

# [!DNL Catalog Service] API ガイド

[!DNL Catalog Service] は、Adobe Experience Platform内のデータの位置と系統に関する記録システムです。[!DNL Catalog] は、データ自体にアクセスすることなく、データに関する情報を検索できるメタデータストア（「カタログ」） [!DNL Experience Platform]として機能します。詳しくは、[[!DNL Catalog] 概要](../home.md)を参照してください。

このデベロッパーガイドでは、[!DNL Catalog] API の使用を開始する際に役立つ手順を説明します。次に、[!DNL Catalog]を使用して主要な操作を実行するためのサンプルAPI呼び出しを提供します。

## 前提条件

[!DNL Catalog] では、様々な種類のリソースおよび操作のメタデータを追跡 [!DNL Experience Platform]します。この開発者ガイドでは、以下のリソースの作成と管理に関連する様々な[!DNL Experience Platform]サービスについて、十分に理解している必要があります。

* [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Platform] に使用される標準化されたフレームワーク。
* [バッチ取得](../../ingestion/batch-ingestion/overview.md)[!DNL Experience Platform]： が CSV や Parket などのデータファイルからデータを取得して保存する方法。
* [ストリーミング取り込み](../../ingestion/streaming-ingestion/overview.md):クライアント側とサーバ側のデバイスからデータを [!DNL Experience Platform] 取り込み、リアルタイムで保存する方法。

以下の節では、[!DNL Catalog Service] APIを正しく呼び出すために知る必要がある、または手元に置く必要がある追加情報について説明します。

## API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。サンプル API 呼び出しのドキュメントで使用されている規則については、[!DNL Experience Platform] トラブルシューテングガイドの[サンプル API 呼び出しの読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

## 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

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

このドキュメントでは、[!DNL Catalog] APIを呼び出すために必要な前提条件に関する知識を説明しました。 これで、この開発者ガイドに記載されているサンプル呼び出しに進んで、その手順に従うことができます。

このガイドの例のほとんどは`/dataSets`エンドポイントを使用していますが、この原則は[!DNL Catalog]内の他のエンドポイント（`/batches`や`/accounts`など）に適用できます。 各エンドポイントで使用できるすべての呼び出しと操作の完全なリストについては、『[カタログサービス API リファレンス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)』を参照してください。

[!DNL Catalog] APIとデータ取り込みとの関わり方を示すワークフローについては、[データセット](../datasets/create.md)の作成に関するチュートリアルを参照してください。
