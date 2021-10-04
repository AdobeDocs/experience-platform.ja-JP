---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、トラブルシューティング、API
title: リアルタイム顧客プロファイル API の概要
topic-legacy: guide
type: Documentation
description: プロファイル API の概要ガイドでは、リアルタイム顧客プロファイル API エンドポイントを使用してプロファイルデータに対して基本的な CRUD 操作を実行する際に知っておく必要がある主要な概念と基本機能について説明しています。
exl-id: 7e30610a-a7e7-43ab-a45d-fd84ef6e36ef
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 44%

---

# [!DNL Real-time Customer Profile] API の概要 {#getting-started}

リアルタイム顧客プロファイル API エンドポイントを使用すると、計算済み属性の設定、エンティティへのアクセス、プロファイルデータの書き出し、不要なデータセットやバッチの削除など、プロファイルデータに対して基本的な CRUD 操作を実行できます。

開発者ガイドを使用するには、[!DNL Profile] データの操作に関わる様々なAdobe Experience Platformサービスに関する十分な知識が必要です。 [!DNL Real-time Customer Profile] API の使用を開始する前に、次のサービスのドキュメントを確認してください。

* [[!DNL Real-time Customer Profile]](../home.md):複数のソースからの集計データに基づいて、統合された顧客プロファイルをリアルタイムで提供します。
* [[!DNL Adobe Experience Platform Identity Service]](../../identity-service/home.md):デバイスやシステム間で ID を結び付けることで、顧客とその行動をより良く把握できます。
* [[!DNL Adobe Experience Platform Segmentation Service]](../../segmentation/home.md):リアルタイム顧客プロファイルデータからオーディエンスセグメントを作成できます。
* [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):Platform が顧客体験データを整理する際に使用する標準化されたフレームワーク。
* [[!DNL Sandboxes]](../../sandboxes/home.md): [!DNL Experience Platform] は、単一のインスタンスを別々の仮想環境に分 [!DNL Platform] 割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Profile] API エンドポイントを正しく呼び出すために知っておく必要がある追加情報を示します。

## API 呼び出し例の読み取り

[!DNL Real-time Customer Profile] API ドキュメントには、リクエストを適切に書式設定する方法を示す API 呼び出しの例が記載されています。 これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

## 必須ヘッダー

また、API ドキュメントでは、 エンドポイントを正しく呼び出すために、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了している必要があります。[!DNL Platform]次に示すように、[!DNL Experience Platform] API 呼び出しにおける各必須ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

リクエスト本文にペイロードを持つすべてのリクエスト（POST、PUT、PATCH 呼び出しなど）には、`Content-Type` ヘッダーが含まれている必要があります。各呼び出しに固有の受け入れられた値は、呼び出しパラメーターに指定されます。

## 次の手順

[!DNL Real-time Customer Profile] API を使用した呼び出しを開始するには、使用可能なエンドポイントガイドの 1 つを選択します。
