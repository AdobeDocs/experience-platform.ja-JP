---
keywords: Experience Platform；ホーム；人気の高いトピック；カタログサービス；カタログ；カタログサービス；カタログ
solution: Experience Platform
title: カタログサービス API ガイド
description: カタログサービス API を使用すると、開発者はAdobe Experience Platformでデータセットメタデータを管理できます。 このガイドに従って、API を使用した主な操作の実行方法を学習します。
exl-id: 812fcdae-ed0e-4f2b-84d7-26f2f79e71b9
source-git-commit: 07451b8ab4bcb7ca43ad0c8a821478b2c9682894
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 58%

---

# [!DNL Catalog Service] API ガイド

[!DNL Catalog Service] は、Adobe Experience Platform 内のデータの場所と系列の記録システムです。[!DNL Catalog] は、内のデータに関する情報を検索できるメタデータストアまたは「カタログ」として機能します。 [!DNL Experience Platform]（データ自体にアクセスする必要はありません）。 詳しくは、[[!DNL Catalog] 概要](../home.md)を参照してください。

このデベロッパーガイドでは、[!DNL Catalog] API を使い始めるのに役立つ手順を説明します。次に、 [!DNL Catalog].

## 前提条件

[!DNL Catalog] では、様々な種類のリソースおよび操作のメタデータを内でトラッキングします。 [!DNL Experience Platform]. この開発者ガイドでは、 [!DNL Experience Platform] 以下のリソースの作成と管理に関わるサービス：

* [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：[!DNL Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。
* [バッチ取得](../../ingestion/batch-ingestion/overview.md)[!DNL Experience Platform]： が CSV や Parket などのデータファイルからデータを取得して保存する方法。
* [ストリーミング取り込み](../../ingestion/streaming-ingestion/overview.md):方法 [!DNL Experience Platform] クライアントサイドおよびサーバーサイドのデバイスからデータをリアルタイムで取得し、保存します。

以下の節では、 [!DNL Catalog Service] API

## API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

## 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* Authorization： Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次のような追加ヘッダーが必要です。

* Content-Type：application/json

## のベストプラクティス [!DNL Catalog] API 呼び出し

に対してGETリクエストを実行する際 [!DNL Catalog] API のベストプラクティスは、必要なオブジェクトとプロパティのみを返すために、リクエストにクエリパラメーターを含めることです。 フィルターを適用しないリクエストの応答ペイロードのサイズは 3 GB に達っすることがあり、全体的なパフォーマンスが低下する可能性があります。

特定のオブジェクトを表示するには、リクエストパスに ID を含めるか、または `properties` や `limit` などのクエリーパラメーターを使用して応答をフィルターします。フィルターは、ヘッダーおよびクエリーパラメーターとして渡すことができ、クエリーパラメーターとして渡されたフィルターが優先されます。詳しくは、[カタログデータのフィルター](filter-data.md)に関するドキュメントを参照してください。

一部のクエリが API に大きな負荷をかける可能性があるので、グローバル制限が [!DNL Catalog] のクエリを参照して、ベストプラクティスをさらにサポートします。

## 次の手順

このドキュメントでは、 [!DNL Catalog] API これで、この開発者ガイドに記載されているサンプル呼び出しに進んで、その手順に従うことができます。

このガイドの例のほとんどでは、 `/dataSets` エンドポイントに適用されますが、原則は、 [!DNL Catalog] ( `/batches`) をクリックします。 各エンドポイントで使用できるすべての呼び出しと操作の完全なリストについては、『[カタログサービス API リファレンス](https://www.adobe.io/experience-platform-apis/references/catalog/)』を参照してください。

を [!DNL Catalog] API はデータ取得に関わっています。詳しくは、 [データセットの作成](../datasets/create.md).
