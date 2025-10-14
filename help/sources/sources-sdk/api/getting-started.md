---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
solution: Experience Platform
title: セルフサービスソースの概要（バッチ SDK）
description: このドキュメントでは、セルフサービスソース（Batch SDK）を使用して新しいソースを作成する前に知っておく必要がある前提条件の概要を説明します。
exl-id: ba131442-ff20-4854-87fe-918aa313382d
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 32%

---

# セルフサービスソースの概要（バッチ SDK）

セルフサービスソース（バッチ SDK）を使用すると、独自の REST ベースのソースを統合して、バッチデータをAdobe Experience Platformに取り込むことができます。 このドキュメントでは、[[!DNL Flow Service] API](https://developer.adobe.com/experience-platform-apis/references/flow-service/) を呼び出す前に知っておく必要があるコア概念の概要を説明します。

## 前提条件

セルフサービスソース（バッチ SDK）を使用するには、Adobe Experience Platform ソースをプロビジョニングされた組織サンドボックスへのアクセス権があることを確認する必要があります。

このガイドには、Adobe Experience Platformの次のコンポーネントに関する十分な知識も必要です。

* [&#x200B; ソース &#x200B;](../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [&#x200B; サンドボックス &#x200B;](../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## API 呼び出し例の読み取り

セルフサービスソース（バッチ SDK）と [!DNL Flow Service] API ドキュメントには、API 呼び出しの例とリクエストの形式を指定する方法が示されています。 これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

## 必須ヘッダーの値の収集

Experience Platform API を呼び出すには、まず[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

[!DNL Flow Service] に属するリソースを含む、Experience Platformのすべてのリソースは、特定の仮想サンドボックスに分離されます。 Experience Platform API へのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>Experience Platformのサンドボックスについて詳しくは、[&#x200B; サンドボックスのドキュメント &#x200B;](../../../sandboxes/home.md) を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 次の手順

セルフサービスソース（バッチ SDK）を使用した新しいソースの作成を開始するには、[&#x200B; 新しいソースの作成 &#x200B;](./create.md) に関するチュートリアルを参照してください。
