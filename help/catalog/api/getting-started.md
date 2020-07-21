---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Catalog Service開発ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 0%

---


# [!DNL Catalog Service] 開発ガイド

[!DNL Catalog Service] は、Adobe Experience Platform内のデータの場所と系列のレコードシステムです。 [!DNL Catalog] は、データ自体にアクセスすることなく、データに関する情報を検索できるメタデータストア（「カタログ」） [!DNL Experience Platform]として機能します。 See the [Catalog overview](../home.md) for more information.

この開発者ガイドでは、 [!DNL Catalog] APIを使用する開始に役立つ手順を説明します。 次に、を使用して主要な操作を実行するためのサンプルAPI呼び出しを提供 [!DNL Catalog]します。

## 前提条件

[!DNL Catalog] では、様々な種類のリソースおよび操作のメタデータを追跡 [!DNL Experience Platform]します。 この開発者ガイドでは、以下のリソースの作成と管理に関連する様々な [!DNL Experience Platform] サービスについて、十分に理解している必要があります。

* [!DNL Experience Data Model (XDM)](../../xdm/home.md): 顧客体験データを [!DNL Platform] 整理するための標準化されたフレームワーク。
* [バッチインジェスト](../../ingestion/batch-ingestion/overview.md): CSVやParketなどのデータファイルからデータを取り込んで保存する方法。 [!DNL Experience Platform]
* [ストリーミング取り込み](../../ingestion/streaming-ingestion/overview.md): クライアント側とサーバ側のデバイスからデータをリアルタイムで取り込み、保存する方法。 [!DNL Experience Platform]

以下の節では、 [!DNL Catalog Service] APIを正しく呼び出すために知る必要がある、または手元にある情報について説明します。

## サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

## 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>のサンドボックスについて詳し [!DNL Platform]くは、 [Sandboxの概要ドキュメントを参照してください](../../sandboxes/home.md)。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

* Content-Type: application/json

## API呼び出しのベストプラクティス [!DNL Catalog]

APIへのGETリクエストを実行する場合、最良の方法は、必要なオブジェクトとプロパティのみを返すために、リクエストにクエリパラメーターを含めることです。 [!DNL Catalog] フィルターを適用しない要求は、応答ペイロードのサイズが過去3 GBに達する可能性があり、全体のパフォーマンスが低下する可能性があります。

リクエストパスにIDを含めることで特定のオブジェクトを表示したり、 `properties` やなどのクエリパラメーターを使用して応答をフィルターしたりでき `limit` ます。 フィルターーは、ヘッダーとして、またはクエリーパラメーターとして渡すことができます。クエリーパラメーターとして渡されるパラメーターは、優先されます。 詳しくは、カタログデータの [フィルタリングに関するドキュメント](filter-data.md) を参照してください。

一部のクエリではAPIに大きな負荷がかかる場合があるので、ベストプラクティスをさらにサポートするために、 [!DNL Catalog] クエリにグローバル制限が実装されています。

## 次の手順

このドキュメントでは、 [!DNL Catalog] APIを呼び出すために必要な前提条件に関する知識を説明しました。 これで、この開発者ガイドに記載されているサンプル呼び出しに進み、それらの指示に従うことができます。

このガイドの例のほとんどはエンドポイントを使用していますが、 `/dataSets` 原則は内部の他のエンドポイント [!DNL Catalog] ( `/batches` や `/accounts`など)に適用できます。 各エンドポイントで使用可能なすべての呼び出しと操作の完全なリストについては、 [カタログサービスAPIリファレンスを参照してください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) 。

データ取り込みと [!DNL Catalog] APIの関係を示すワークフローについては、データセットの [作成に関するチュートリアルを参照してください](../datasets/create.md)。
