---
keywords: Experience Platform;プロファイル;リアルタイム顧客プロファイル;トラブルシューティング;API
title: リアルタイム顧客プロファイル API の概要
type: Documentation
description: Profile API 入門ガイドでは、リアルタイム顧客プロファイル API エンドポイントを使用してプロファイルデータに対する基本的な CRUD 操作を実行するために知っておく必要がある、主な概念と基本機能の概要を説明します。
role: Developer
exl-id: 7e30610a-a7e7-43ab-a45d-fd84ef6e36ef
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 53%

---

# [!DNL Real-Time Customer Profile] API の概要 {#getting-started}

リアルタイム顧客プロファイル API エンドポイントを使用すると、計算属性の設定、エンティティへのアクセス、プロファイルデータの書き出し、不要なデータセットやバッチの削除など、プロファイルデータに対して基本的な CRUD 操作を実行できます。

このデベロッパーガイドを使用するには、[!DNL Profile] データの操作に関わる様々なAdobe Experience Platform サービスについて実際に理解しておく必要があります。 [!DNL Real-Time Customer Profile] API の使用を開始する前に、次のサービスのドキュメントを確認してください。

* [[!DNL Real-Time Customer Profile]](../home.md)：複数のソースから集計したデータに基づいて、統合された顧客プロファイルをリアルタイムで提供します。
* [[!DNL Adobe Experience Platform Identity Service]](../../identity-service/home.md)：デバイスやシステム間で ID を橋渡しすることで、顧客とその行動をより確実に把握することができます。
* [[!DNL Adobe Experience Platform Segmentation Service]](../../segmentation/home.md)：リアルタイム顧客プロファイルデータからオーディエンスを作成できます。
* [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：Experience Platform が顧客体験データを整理するための標準的なフレームワーク。
* [[!DNL Sandboxes]](../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、API エンドポイントを正しく呼び出すために知っておく必要がある追加情報 [!DNL Profile] 示します。

## API 呼び出し例の読み取り

[!DNL Real-Time Customer Profile] API ドキュメントには、API 呼び出しの例とリクエストの適切な形式を示す方法が記載されています。 これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

## 必須ヘッダー

また、API ドキュメントでは、 エンドポイントを正しく呼び出すために、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了している必要があります。[!DNL Experience Platform]次に示すように、[!DNL Experience Platform] API 呼び出しにおける各必須ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Experience Platform] API へのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

[!DNL Experience Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

リクエスト本文にペイロードを持つすべてのリクエスト（POST、PUT、PATCH 呼び出しなど）には、`Content-Type` ヘッダーが含まれている必要があります。各呼び出しに固有の受け入れられた値は、呼び出しパラメーターに指定されます。

## 次の手順

[!DNL Real-Time Customer Profile] API を使用した呼び出しを開始するには、使用可能なエンドポイントガイドの 1 つを選択します。
