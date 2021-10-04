---
keywords: Experience Platform;ホーム;人気のトピック;DULE;dule
solution: Experience Platform
title: Policy Service API の概要
topic-legacy: developer guide
description: Policy Service API を使用すると、Adobe Experience Platform データガバナンスに関連する様々なリソースを作成および管理できます。このドキュメントでは、ポリシーサービス API を呼び出す前に知っておく必要があるコア概念の概要を説明します。
exl-id: 5539976c-8433-45af-a147-2ab82ae308b2
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 100%

---

# [!DNL Policy Service] API の概要

[!DNL Policy Service] API を使用すると、Adobe Experience Platform [!DNL Data Governance]に関する様々なリソースを作成および管理できます。このドキュメントでは、[!DNL Policy Service] API を呼び出す前に知っておく必要があるコア概念の概要を説明します。

## 前提条件

デベロッパーガイドを使用するには、データガバナンス機能の操作に関わる様々な [!DNL Experience Platform] サービスについて実際に理解しておく必要があります。[!DNL Policy Service API] の使用を開始する前に、次のサービスのドキュメントを確認してください。

* [[!DNL Data Governance]](../home.md)：[!DNL Experience Platform] がデータ使用のコンプライアンスを強制するフレームワーク。
* [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Experience Platform] に使用される標準化されたフレームワーク。
* [[!DNL Real-time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
* [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

## API 呼び出し例の読み取り

[!DNL Policy Service] API ドキュメントには、API 呼び出しの例とリクエストの形式を指定する方法が示されています。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

## 必須ヘッダー

また、API ドキュメントでは、 エンドポイントを正しく呼び出すために、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了している必要があります。[!DNL Platform]次に示すように、[!DNL Experience Platform] API 呼び出しにおける各必須ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Data Governance]に属するリソースを含む、[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されます。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## コアリソースとカスタムリソース

[!DNL Policy Service] API 内では、ポリシーとマーケティングアクションはすべて、`core` リソースまたは `custom` リソースと呼ばれます。

`core` リソースとは、アドビが定義および保守するものです。 `custom` リソースとは、お客様の組織が作成および保守するものであり、お客様の IMS 組織のみに表示される一意のリソースです。このため、`core` リソースに対して許可される操作は、リストと参照の操作（`GET`）のみです。一方、`custom` リソースに対しては、リスト、参照、および更新の操作（`POST`、`PUT`、`PATCH`、`DELETE`）を使用できます。

## 次の手順

Policy Service API を使用した呼び出しを開始するには、使用可能なエンドポイントガイドの 1 つを選択します。
