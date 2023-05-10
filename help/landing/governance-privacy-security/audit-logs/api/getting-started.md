---
title: 監査クエリ API の概要
description: 監査クエリ API を使用すると、様々なAdobe Experience Platform機能の指標データを取得できます。 このドキュメントでは、監査クエリ API を呼び出す前に知っておく必要があるコア概念の概要を説明します。
exl-id: 20eab0a8-98f7-4fee-8f91-88324e54ab18
source-git-commit: c2c5778e0a3fff7f488ad7a672123c813cca59f1
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 49%

---

# 監査クエリ API の概要

Adobe Experience Platformでは、監査イベントログの形式で、様々なサービスや機能のユーザーアクティビティを監査できます。 ログに記録される各アクションには、アクションのタイプ、日時、アクションを実行したユーザーの電子メール ID、アクションのタイプに関連する追加の属性を示すメタデータが含まれます。

Audit Query API を使用すると、監査イベントログの形式で、様々なサービスや機能のユーザーアクティビティを監査できます。 このドキュメントでは、監査クエリ API を呼び出す前に知っておく必要があるコア概念の概要を説明します。

## 前提条件

監査イベントを管理するには、 **[!UICONTROL ユーザーアクティビティログを表示]** アクセス制御権限が付与されている ( [!UICONTROL データガバナンス] カテゴリ ) です。 Platform 機能の個々の権限を管理する方法については、 [アクセス制御ドキュメント](../../../../access-control/home.md).

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。API 呼び出し例のドキュメントで使用される表記について詳しくは、『Experience Platform トラブルシューティングガイド』の [API 呼び出し例の読み方](../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必要なヘッダーの値の収集

このガイドでは、Platform API を正しく呼び出すために[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了している必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key： `{API_KEY}`
* x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。へのすべてのリクエスト [!DNL Platform] API には、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。 [!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../../../sandboxes/home.md)を参照してください。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次の追加のヘッダーが必要です。

* Content-Type: application/json

## 次の手順

を使用して呼び出しを開始するには、以下を実行します。 [!DNL Audit Query] API（を参照） [イベントエンドポイントガイド](./events.md) そして [書き出しエンドポイントガイド](./export.md).
