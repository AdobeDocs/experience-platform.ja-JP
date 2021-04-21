---
keywords: Experience Platform；ホーム；人気のあるトピック；DULE；スケジュール
solution: Experience Platform
title: Policy Service APIの使い始めに
topic-legacy: developer guide
description: Policy Service APIを使用すると、Adobe Experience Platformデータガバナンスに関する様々なリソースを作成および管理できます。 このドキュメントでは、ポリシーサービス API を呼び出す前に知っておく必要があるコア概念の概要を説明します。
exl-id: 5539976c-8433-45af-a147-2ab82ae308b2
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 33%

---

# [!DNL Policy Service] API使用の手引き

[!DNL Policy Service] APIを使うと、Adobe Experience Platform[!DNL Data Governance]に関連する様々なリソースを作成し管理できます。 このドキュメントでは、[!DNL Policy Service] APIを呼び出す前に知っておく必要があるコア概念の紹介を行います。

## 前提条件

開発者ガイドを使用するには、データガバナンス機能の操作に関わる様々な[!DNL Experience Platform]サービスに関する作業を理解しておく必要があります。 [!DNL Policy Service API]との連携を開始する前に、次のサービスのドキュメントを確認してください。

* [[!DNL Data Governance]](../home.md):データ使用のコンプライアンスを [!DNL Experience Platform] 強制するフレームワーク。
* [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Experience Platform] に使用される標準化されたフレームワーク。
* [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [サンドボックス](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

## API 呼び出し例の読み取り

[!DNL Policy Service] APIドキュメントには、リクエストをフォーマットする方法を示すAPI呼び出しの例が記載されています。 この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。サンプル API 呼び出しのドキュメントで使用されている規則については、[!DNL Experience Platform] トラブルシューテングガイドの[サンプル API 呼び出しの読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

## 必須ヘッダー

また、API ドキュメントでは、 エンドポイントを正しく呼び出すために、[認証に関するチュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了している必要があります。[!DNL Platform]次に示すように、認証チュートリアルで、[!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定します。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Data Governance]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform]のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含んだすべてのリクエストには、以下の追加ヘッダーが必要です。

* `Content-Type: application/json`

## コアリソースとカスタムリソース

[!DNL Policy Service] API内では、すべてのポリシーとマーケティングアクションが`core`または`custom`リソースと呼ばれます。

`core` リソースとは、Adobeが定義および保守するものです。 `custom` リソースとは、組織が作成および保守するものであり、したがって、IMS組織のみに表示される一意のリソースです。このため、`core` リソースに対して許可される操作は、リストと参照の操作（`GET`）のみです。一方、`custom` リソースに対しては、リスト、参照、および更新の操作（`POST`、`PUT`、`PATCH`、`DELETE`）を使用できます。

## 次の手順

Policy Service APIを使用した呼び出しを開始するには、使用可能なエンドポイントガイドの1つを選択します。
