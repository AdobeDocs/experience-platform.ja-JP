---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、トラブルシューティング、API
title: データ取得 API の使用の手引き
type: Documentation
description: 『データ取得 API 入門ガイド』では、API を使用してデータをExperience Platformに取り込む前に知っておく必要がある主要な概念と基本機能の概要を説明しています。
exl-id: 0653de2b-3268-478b-a23f-c458b6d3df7c
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 51%

---

# データ取得 API の使用の手引き {#getting-started}

データ取得 API エンドポイントを使用して、基本的な CRUD 操作を実行し、Adobe Experience Platformでデータを取り込むことができます。

API ガイドを使用するには、データの操作に関わる複数のAdobe Experience Platformサービスに関する十分な知識が必要です。 データ取得 API を使用する前に、次のサービスのドキュメントを確認してください。

* [バッチ取得](./overview.md)：データをバッチファイルとして Adobe Experience Platform に取得することができます。
* [[!DNL Real-time Customer Profile]](../home.md):複数のソースからの集計データに基づいて、統合された顧客プロファイルをリアルタイムで提供します。
* [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)： Platform が顧客体験データを整理するための標準的なフレームワーク。
* [[!DNL Sandboxes]](../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

以下の節では、への呼び出しを正しくおこなうために知っておく必要がある追加情報を示します。 [!DNL Profile] API エンドポイント。

## API 呼び出し例の読み取り

データ取得 API のドキュメントには、API 呼び出しの例とリクエストの形式を適切に設定する方法が記載されています。 これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

## 必須ヘッダー

また、API ドキュメントでは、 エンドポイントを正しく呼び出すために、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了している必要があります。[!DNL Platform]次に示すように、[!DNL Experience Platform] API 呼び出しにおける各必須ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

リクエスト本文にペイロードを持つすべてのリクエスト（POST、PUT、PATCH 呼び出しなど）には、`Content-Type` ヘッダーが含まれている必要があります。各呼び出しに固有の受け入れられた値は、呼び出しパラメーターに指定されます。

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。へのリクエスト [!DNL Platform] API には、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。
