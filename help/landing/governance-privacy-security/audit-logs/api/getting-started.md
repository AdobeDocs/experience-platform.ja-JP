---
title: 監査クエリ API の概要
description: Audit Query API を使用すると、様々なAdobe Experience Platform機能の指標データを取得できます。 このドキュメントでは、監査クエリ API を呼び出す前に知っておく必要があるコア概念の概要を説明します。
role: Developer
feature: Audits, API
exl-id: 20eab0a8-98f7-4fee-8f91-88324e54ab18
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '391'
ht-degree: 35%

---

# 監査クエリ API の概要

Adobe Experience Platformでは、様々なサービスおよび機能に関するユーザーアクティビティを監査イベントログの形式で監査できます。 ログに記録される各アクションには、アクションのタイプ、日時、アクションを実行したユーザーのメール ID、アクションのタイプに関連する追加の属性を示すメタデータが含まれます。

Audit Query API を使用すると、様々なサービスおよび機能に関するユーザーアクティビティを監査イベントログの形式で監査できます。 このドキュメントでは、監査クエリ API を呼び出す前に知っておく必要があるコア概念の概要を説明します。

## 前提条件

監査イベントを管理するには、**[!UICONTROL ユーザーアクティビティログを表示]** アクセス制御権限を付与する必要があります（[!UICONTROL  データガバナンス ] カテゴリの下にあります）。 Experience Platform機能の個々の権限を管理する方法については、[ アクセス制御ドキュメント ](../../../../access-control/home.md) を参照してください。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。API 呼び出し例のドキュメントで使用される表記について詳しくは、『Experience Platform トラブルシューティングガイド』の [API 呼び出し例の読み方](../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必要なヘッダーの値の収集

このガイドでは、Experience Platform API を正しく呼び出すために、[ 認証に関するチュートリアル ](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja) を完了している必要があります。 認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Experience Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。 [!DNL Experience Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../../../sandboxes/home.md)を参照してください。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次の追加のヘッダーが必要です。

* Content-Type: application/json

## 次の手順

[!DNL Audit Query] API を使用した呼び出しを開始するには、[ イベントエンドポイントガイド ](./events.md) および [ 書き出しエンドポイントガイド ](./export.md) を参照してください。
