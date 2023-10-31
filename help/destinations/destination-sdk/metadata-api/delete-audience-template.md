---
description: このページでは、Adobe Experience Platform Destination SDK を通じて、既存のオーディエンステンプレートを削除するために使用される API 呼び出しの例を示します。
title: オーディエンステンプレートの削除
exl-id: 6eb07e3c-3269-4368-9b11-04bd993cc4ab
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 100%

---

# オーディエンステンプレートの削除

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/audience-templates`

このページでは、`/authoring/audience-templates` API エンドポイントを使用して、オーディエンステンプレートを削除するために使用できる API リクエストおよびペイロードの例を示します。

このエンドポイントを通じて設定できる機能について詳しくは、[オーディエンスメタデータ管理](../functionality/audience-metadata-management.md)を参照してください。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## オーディエンステンプレート API 操作の概要 {#get-started}

続行する前に、「[はじめる前に](../getting-started.md)」を参照し、API の呼び出しを正常に行うために必要となる重要な情報（必要な宛先オーサリング権限および必要なヘッダーの取得方法など）を確認してください。

## オーディエンステンプレートの削除 {#delete}

削除したいオーディエンステンプレートの `{INSTANCE_ID}` で `/authoring/audience-templates` エンドポイントに `DELETE` リクエストを行うことで、[既存の](create-audience-template.md)オーディエンステンプレートを削除できます。

既存のオーディエンステンプレートおよびその関連する `{INSTANCE_ID}` を取得するには、[オーディエンステンプレートの取得](retrieve-audience-template.md)に関する記事を参照してください。

**API 形式**

```http
DELETE /authoring/audience-templates/{INSTANCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{INSTANCE_ID}` | 削除するオーディエンステンプレートの `ID`。 |

+++リクエスト

```shell
curl -X DELETE https://platform.adobe.io/data/core/activation/authoring/audience-templates/{INSTANCE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

+++

+++応答

応答が成功すると、HTTP ステータス 200 が、空の HTTP 応答と共に返されます。

+++

## API エラー処理 {#error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順 {#next-steps}

このドキュメントでは、`/authoring/audience-templates` API エンドポイントを使用した、オーディエンステンプレートの削除方法を確認しました。この手順が宛先設定プロセスのどこに当てはまるかを把握するには、[Destination SDK を使用した宛先の設定方法](../guides/configure-destination-instructions.md)を参照してください。
