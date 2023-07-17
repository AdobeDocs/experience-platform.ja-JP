---
description: 宛先テスト API を使用して、宛先用のテストメッセージ変換テンプレートを生成する方法を説明します。
title: サンプルメッセージ変換テンプレートの生成
exl-id: d18a06f7-0c3a-4b4d-a7d5-011690d00e2c
source-git-commit: d6402f22ff50963b06c849cf31cc25267ba62bb1
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 100%

---


# サンプルメッセージ変換テンプレートの生成 {#get-sample-template-api-operations}

>[!IMPORTANT]
>
>**API エンドポイント**：`https://platform.adobe.io/data/core/activation/authoring/testing/template/sample`

このページでは、宛先用に[メッセージ変換テンプレート](../../functionality/destination-server/message-format.md#using-templating)を生成するために、`/authoring/testing/template/sample` API エンドポイントを使用して実行できる、すべての API 操作を一覧表示および説明しています。このエンドポイントでサポートされる機能の説明については、[テンプレートの作成](create-template.md)を参照してください。

## サンプルテンプレート API 操作の概要 {#get-started}

続行する前に、「[はじめる前に](../../getting-started.md)」を参照し、API の呼び出しを正常に行うために必要となる重要な情報（必要な宛先オーサリング権限および必要なヘッダーの取得方法など）を確認してください。

## サンプルテンプレートの取得 {#generate-sample-template}

`authoring/testing/template/sample/` エンドポイントに対して GET リクエストを行い、作成しているテンプレートに基づいて宛先設定の宛先 ID を指定することで、サンプルテンプレートを取得できます。

>[!TIP]
>
>* ここで使用する必要がある宛先 ID は、宛先設定に対応した `instanceId` で、`/destinations` エンドポイントを使用して作成されています。詳しくは、[宛先設定の取得](../../authoring-api/destination-configuration/retrieve-destination-configuration.md)を参照してください。

**API 形式**

```http
GET authoring/testing/template/sample/{DESTINATION_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{DESTINATION_ID}` | メッセージ変換テンプレートを生成している宛先設定の ID。 |

**リクエスト**

以下のリクエストは、ペイロードで提供されるパラメーター設定に基づいて、新しいサンプルテンプレートを生成します。

```shell
curl --location --request GET 'https://platform.adobe.io/data/core/activation/authoring/testing/template/sample/5114d758-ce71-43ba-b53e-e2a91d67b67f' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答が成功すると、HTTP ステータス 200 が、想定されるデータ形式に一致するように編集できるサンプルテンプレートと共に返されます。

指定する宛先 ID が、[ベストエフォート集計](../../functionality/destination-configuration/aggregation-policy.md)と集計ポリシーの `maxUsersPerRequest=1` を含む宛先設定に対応する場合、リクエストは、以下に類似したサンプルテンプレートを返します。

```python
{#- THIS is an example template for a single profile -#}
{#- A '-' at the beginning or end of a tag removes all whitespace on that side of the tag. -#}
{
    "identities": [
    {%- for idMapEntry in input.profile.identityMap -%}
    {%- set namespace = idMapEntry.key -%}
        {%- for identity in idMapEntry.value %}
        {
            "type": "{{ namespace }}",
            "id": "{{ identity.id }}"
        }{%- if not loop.last -%},{%- endif -%}
        {%- endfor -%}{%- if not loop.last -%},{%- endif -%}
    {% endfor %}
    ],
    "AdobeExperiencePlatformSegments": {
        "add": [
        {%- for segment in input.profile.segmentMembership.ups | added %}
            "{{ segment.key }}"{%- if not loop.last -%},{%- endif -%}
        {% endfor %}
        ],
        "remove": [
        {#- Alternative syntax for filtering audiences by status: -#}
        {% for segment in removedSegments(input.profile.segmentMembership.ups) %}
            "{{ segment.key }}"{%- if not loop.last -%},{%- endif -%}
        {% endfor %}
        ]
    }
}
```

指定する宛先 ID が、[設定可能な集計](../../functionality/destination-configuration/aggregation-policy.md#configurable-aggregation)または[ベストエフォート集計](../../functionality/destination-configuration/aggregation-policy.md#best-effort-aggregation)を含み、`maxUsersPerRequest` が 1 より大きい宛先サーバーテンプレートに対応する場合、リクエストは、以下に類似したサンプルテンプレートを返します。

```python
{#- THIS is an example template for multiple profiles -#}
{#- A '-' at the beginning or end of a tag removes all whitespace on that side of the tag. -#}
{
    "profiles": [
    {%- for profile in input.profiles %}
        {
            "identities": [
            {%- for idMapEntry in profile.identityMap -%}
            {%- set namespace = idMapEntry.key -%}
                {%- for identity in idMapEntry.value %}
                {
                    "type": "{{ namespace }}",
                    "id": "{{ identity.id }}"
                }{%- if not loop.last -%},{%- endif -%}
                {%- endfor -%}{%- if not loop.last -%},{%- endif -%}
            {% endfor %}
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                {%- for segment in profile.segmentMembership.ups | added %}
                    "{{ segment.key }}"{%- if not loop.last -%},{%- endif -%}
                {% endfor %}
                ],
                "remove": [
                {#- Alternative syntax for filtering audiences by status: -#}
                {% for segment in removedSegments(profile.segmentMembership.ups) %}
                    "{{ segment.key }}"{%- if not loop.last -%},{%- endif -%}
                {% endfor %}
                ]
            }
        }{%- if not loop.last -%},{%- endif -%}
    {% endfor %}
    ]
}
```

## API エラー処理 {#api-error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順 {#next-steps}

このドキュメントでは、`/authoring/testing/template/sample` API エンドポイントを使用した、メッセージ変換テンプレートの生成方法を確認しました。次に、[レンダリングテンプレート API エンドポイント](render-template-api.md)を使用して、テンプレートに基づいて書き出されたプロファイルを生成し、それらを宛先で想定されているデータ形式と比較できます。
