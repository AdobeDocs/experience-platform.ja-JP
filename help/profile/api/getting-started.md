---
keywords: Experience Platform;プロファイル；リアルタイム顧客プロファイル；トラブルシューティング；API
title: リアルタイム顧客プロファイルAPI使用の手引き
topic: ガイド
type: ドキュメント
description: プロファイルAPIの概要ガイドでは、プロファイルデータに対して基本的なCRUD操作を実行する際にリアルタイム顧客プロファイルAPIエンドポイントを使用する場合に知っておく必要がある、主要な概念と基本的な機能について説明します。
translation-type: tm+mt
source-git-commit: 126b3d1cf6d47da73c6ab045825424cf6f99e5ac
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 25%

---


# [!DNL Real-time Customer Profile] API {#getting-started}の使い始めに

リアルタイム顧客プロファイルAPIエンドポイントを使用すると、計算済み属性の設定、エンティティへのアクセス、プロファイルデータの書き出し、不要なデータセットやバッチの削除など、プロファイルデータに対する基本的なCRUD操作を実行できます。

開発者ガイドを使用するには、[!DNL Profile]データの操作に関与する様々なAdobe Experience Platformサービスに関する作業的な理解が必要です。 [!DNL Real-time Customer Profile] APIの使用を開始する前に、次のサービスのドキュメントを確認してください。

* [[!DNL Real-time Customer Profile]](../home.md):複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
* [[!DNL Adobe Experience Platform Identity Service]](../../identity-service/home.md):デバイスとシステム間でIDをブリッジ化することで、顧客と行動をより良く表示できます。
* [[!DNL Adobe Experience Platform Segmentation Service]](../../segmentation/home.md):リアルタイム顧客プロファイルデータからオーディエンスセグメントを作成できます。
* [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):プラットフォームが顧客体験データを編成する際に使用する標準化されたフレームワーク。
* [[!DNL Sandboxes]](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、[!DNL Profile] APIエンドポイントの呼び出しを正常に行うために知っておく必要がある追加情報について説明します。

## API 呼び出し例の読み取り

[!DNL Real-time Customer Profile] APIドキュメントには、リクエストを正しくフォーマットする方法を示すAPI呼び出しの例が記載されています。 この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

## 必須ヘッダー

また、API ドキュメントでは、 エンドポイントを正しく呼び出すために、[認証に関するチュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了している必要があります。[!DNL Platform]次に示すように、認証チュートリアルで、[!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定します。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソースは、特定の仮想サンドボックスに分離されています。 [!DNL Platform] APIへの要求には、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

[!DNL Platform]のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメント](../../sandboxes/home.md)を参照してください。

リクエスト本文にペイロードを持つすべてのリクエスト（POST、PUT、PATCH 呼び出しなど）には、`Content-Type` ヘッダーが含まれている必要があります。各呼び出しに固有の受け入れられた値は、呼び出しパラメーターに指定されます。

## 次の手順

[!DNL Real-time Customer Profile] APIを使用した呼び出しを開始するには、使用可能なエンドポイントガイドの1つを選択します。