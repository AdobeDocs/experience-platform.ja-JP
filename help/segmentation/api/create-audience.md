---
title: オーディエンス API エンドポイントの作成
description: API を使用して外部オーディエンスのメタデータを作成する方法を説明します。
hide: true
hidefromtoc: true
exl-id: e841a5f6-f406-4e1d-9e8a-acb861ba6587
source-git-commit: bf90b09693c7b9b7d3ad6ccc6940d255bf7bf4cb
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 35%

---

# オーディエンスエンドポイントを作成

POST `/audiences` エンドポイントは、外部オーディエンスのメタデータを作成するために使用できます。これにより、オーディエンスを Audience Portal に表示できます。 オーディエンスの取り込みが、バッチ取り込みなどの別のサービスで管理される場合は、このエンドポイントを使用する必要があります。

## はじめに

>[!IMPORTANT]
>
>このガイドのエンドポイントには、`/core/ais` ではなく、`/core/ups` がプレフィックスとして付けられています。

Experience Platform API を使用するには、[&#x200B; 認証チュートリアル &#x200B;](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja) を完了している必要があります。 次に示すように、Experience Platform API 呼び出しの必要な各ヘッダーの値は、認証に関するチュートリアルで説明されています。

- Authorization: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Experience Platform] API へのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Experience Platform] でのサンドボックスの使用について詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

**API 形式**

>[!IMPORTANT]
>
>**クエリパラメーターをリクエストの一部として含める** 必要があります `createAudienceMetaOnly=true`。

```http
POST /audiences?createAudienceMetaOnly=true
```

**リクエスト**

>[!IMPORTANT]
>
>API リクエストの一部として **ヘッダーを含める** 必要があります `Accept: application/vnd.adobe.external.audiences+json; version=2`。

```shell
curl -X POST https://platform.adobe.io/core/ais/audiences?createAudienceMetaOnly=true \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'\
 -H 'Accept: application/vnd.adobe.external.audiences+json; version=2'
 -d '{
    "name": "Sample audience name",
    "description" "A sample description for the audience.",
    "namespace": "agora",
    "originName": "Agora_Collaboration"
 }'
```

| プロパティ | タイプ | 説明 |
| -------- | ---- | ----------- |
| `name` | 文字列 | オーディエンスの名前。 |
| `description` | 文字列 | オーディエンスのオプションの説明。 |
| `namespace` | 文字列 | オーディエンスの名前空間。 |
| `originName` | 文字列 | オーディエンスの接触チャネルの名前。 |

**応答**

応答に成功すると、HTTP ステータス 200 と、新しく作成されたオーディエンスに関する情報が返されます。

```json
{
    "name": "Sample audience name",
    "audienceId": "4a815904-f2f9-4237-82fb-55605bcc2ad7"
}
```

| プロパティ | タイプ | 説明 |
| -------- | ---- | ----------- |
| `name` | 文字列 | 作成したオーディエンスの名前。 |
| `audienceId` | 文字列 | 作成したオーディエンスの ID。 |
