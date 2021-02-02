---
keywords: Experience Platform；ホーム；人気のあるトピック；アクセス制御;api；はじめに
solution: Experience Platform
title: アクセス制御開発者ガイド
topic: developer guide
description: Adobe Experience Platform のアクセス制御では、Adobe Admin Console を使用して、様々な Platform 機能のロールと権限を管理できます。以下の節では、Schema Registry API への呼び出しを正常に実行するために必要な追加情報を示しています。
translation-type: tm+mt
source-git-commit: ece2ae1eea8426813a95c18096c1b428acfd1a71
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 64%

---


# [!DNL Access control] 開発ガイド

[!DNL Access control] というの [!DNL Experience Platform] は、 [Adobe Admin Consoleを通して投与されるからだ](https://adminconsole.adobe.com)。この機能は、Admin Console の製品プロファイルを利用して、権限およびサンドボックスを持つユーザーをリンクします。詳しくは、「[アクセス制御の概要](../home.md)」を参照してください。

この開発者ガイドは、[[!DNL Access Control API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/access-control.yaml)にリクエストをフォーマットする方法に関する情報を提供し、以下の操作について説明します。

- [権限名とリソースタイプの一覧表示](./permissions-and-resource-types.md)
- [現在のユーザーに対して効果の高いポリシーの表示](./effective-policies.md)

## はじめに

以下の節では、[!DNL Schema Registry] APIを正しく呼び出すために知っておく必要のある追加情報を紹介します。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

[!DNL Platform] APIを呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。 次に示すように、すべての[!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソースは、特定の仮想サンドボックスに分離されています。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform]のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: application/json

## 次の手順

必要な資格情報を収集したので、開発者ガイドの残りをお読みください。各節では、エンドポイントに関する重要な情報を提供し、CRUD 操作を実行するための API 呼び出しの例を示します。各呼び出しには、一般的な API 形式、必要なヘッダーと適切な形式のペイロードを示すサンプルリクエスト、および正常な呼び 出しのサンプル応答が含まれます。
