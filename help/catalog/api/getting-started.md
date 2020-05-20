---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Catalog Service開発ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: eec5b07427aa9daa44d23f09cfaf1b38f8e811f3
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 0%

---


# Catalog Service開発ガイド

カタログサービスは、Adobe Experience Platform内のデータの場所と系列の記録システムです。 カタログはメタデータストア（「カタログ」）の役割を果たし、データ自体にアクセスする必要なく、エクスペリエンスプラットフォーム内でデータに関する情報を検索できます。 See the [Catalog overview](../home.md) for more information.

この開発者ガイドでは、カタログAPIを使用する開始に役立つ手順を説明します。 次に、Catalogを使用して主要な操作を実行するためのサンプルAPI呼び出しを提供します。

## 前提条件

カタログは、エクスペリエンスプラットフォーム内の様々な種類のリソースおよび操作のメタデータを追跡します。 この開発者ガイドでは、以下のリソースの作成と管理に関連する様々なExperience Platformサービスについて、十分に理解している必要があります。

* [Experience Data Model(XDM)](../../xdm/home.md): プラットフォームが顧客体験データを編成する際に使用する標準化されたフレームワーク。
* [バッチインジェスト](../../ingestion/batch-ingestion/overview.md): エクスペリエンスプラットフォームがCSVやParketなどのデータファイルからデータを取り込んで保存する方法。
* [ストリーミング取り込み](../../ingestion/streaming-ingestion/overview.md): Experience Platformがクライアント側とサーバー側のデバイスからデータをリアルタイムで取り込み、保存する方法。

以下の節では、Catalog Service APIの呼び出しを正常に行うために知る必要がある、または手元にある情報について説明します。

## サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

## 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されています。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、「 [サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

* Content-Type: application/json

## カタログAPI呼び出しのベストプラクティス

カタログAPIへのGETリクエストを実行する場合、必要なオブジェクトとプロパティのみを返すために、リクエストにクエリパラメーターを含めることをお勧めします。 フィルターを適用しない要求は、応答ペイロードのサイズが過去3 GBに達する可能性があり、全体のパフォーマンスが低下する可能性があります。

リクエストパスにIDを含めることで特定のオブジェクトを表示したり、 `properties` やなどのクエリパラメーターを使用して応答をフィルターしたりでき `limit` ます。 フィルターーは、ヘッダーとして、またはクエリーパラメーターとして渡すことができます。クエリーパラメーターとして渡されるパラメーターは、優先されます。 詳しくは、カタログデータの [フィルタリングに関するドキュメント](filter-data.md) を参照してください。

一部のクエリではAPIに大きな負荷がかかる場合があるので、ベストプラクティスをさらにサポートするために、カタログクエリにグローバル制限が実装されています。

## 次の手順

このドキュメントでは、Catalog APIを呼び出すために必要な前提条件に関する知識を説明しました。 これで、この開発者ガイドに記載されているサンプル呼び出しに進み、それらの指示に従うことができます。

このガイドの例のほとんどはエンドポイントを使用していますが、 `/dataSets` 原則はカタログ内の他のエンドポイント( `/batches` や `/accounts`など)に適用できます。 各エンドポイントで使用可能なすべての呼び出しと操作の完全なリストについては、 [カタログサービスAPIリファレンスを参照してください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) 。

カタログAPIとデータ取り込みとの関わり方を示すワークフローについては、データセットの [作成に関するチュートリアルを参照してください](../datasets/create.md)。
