---
description: このページでは、Adobe Experience Platform Destination SDKを通じて既存のオーディエンステンプレートを削除するための API 呼び出しの例を示します。
title: オーディエンステンプレートの削除
source-git-commit: 9e1ae44f83b886f0b5dd5a9fc9cd9b7db6154ff0
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 42%

---


# オーディエンステンプレートの削除

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/audience-templates`

このページの例では、 `/authoring/audience-templates` API エンドポイント。

このエンドポイントを通じて設定できる機能について詳しくは、 [audience metadata management](../functionality/audience-metadata-management.md).

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## オーディエンステンプレート API 操作の概要 {#get-started}

続ける前に「[はじめる前に](../getting-started.md)」を参照し、必要な宛先オーサリング権限および必要なヘッダーの取得方法など、API の呼び出しを正常に行うために必要となる重要な情報を確認してください。

## オーディエンステンプレートの削除 {#delete}

次の項目を削除できます。 [既存](create-audience-template.md) オーディエンステンプレートを作成する `DELETE` にリクエスト `/authoring/audience-templates` endpoint に `{INSTANCE_ID}`を選択します。

既存のオーディエンステンプレートとそれに対応するオーディエンステンプレートを取得するには `{INSTANCE_ID}`に関する記事を参照してください。 [オーディエンステンプレートの取得](retrieve-audience-template.md).

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

リクエストが成功した場合は、空の HTTP 応答とともに HTTP ステータス 200 が返されます。

+++

## API エラー処理 {#error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順 {#next-steps}

このドキュメントを読んだ後、 `/authoring/audience-templates` API エンドポイント。 [Destination SDK を使用して宛先を設定する方法](../guides/configure-destination-instructions.md)を参照して、この手順が宛先を設定するプロセスの中でどのように位置づけられるかを把握します。
