---
description: このページでは、宛先のテストメッセージ変換テンプレートを取得するために、「/authoring/testing/template/sample」 API エンドポイントを使用して実行できるすべての API 操作について説明します。
title: 「Get sample template API」操作
exl-id: d18a06f7-0c3a-4b4d-a7d5-011690d00e2c
source-git-commit: 6dd8a94e46b9bee6d1407e7ec945a722d8d7ecdb
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 2%

---

# 「Get sample template API」操作 {#get=sample-template-api-operations}

>[!IMPORTANT]
>
>**API エンドポイント**: `https://platform.adobe.io/data/core/activation/authoring/testing/template/sample`

このページでは、 `/authoring/testing/template/sample` API エンドポイント（を生成するため） [メッセージ変換テンプレート](./message-format.md#using-templating) を設定します。 このエンドポイントでサポートされる機能については、 [テンプレートを作成](./create-template.md).

## サンプルテンプレート API 操作の概要 {#get-started}

続行する前に、 [入門ガイド](./getting-started.md) を参照してください。

## サンプルテンプレートの取得 {#generate-sample-template}

サンプルテンプレートを取得するには、 `authoring/testing/template/sample/` エンドポイントを作成し、テンプレートを作成する場所に基づいて宛先設定の宛先 ID を指定します。

>[!TIP]
>
>* ここで使用する必要がある宛先 ID は、です。 `instanceId` を使用して作成された、宛先の設定に対応する `/destinations` endpoint. 詳しくは、 [宛先設定 API リファレンス](./destination-configuration-api.md#retrieve-list).


**API 形式**


```http
GET authoring/testing/template/sample/{DESTINATION_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{DESTINATION_ID}` | メッセージ変換テンプレートを生成する宛先設定の ID。 |

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターで設定された新しいサンプルテンプレートを生成します。

```shell
curl --location --request GET 'https://platform.adobe.io/data/core/activation/authoring/testing/template/sample/5114d758-ce71-43ba-b53e-e2a91d67b67f' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

正常な応答は、HTTP ステータス 200 と、期待されるデータ形式に合わせて編集できるサンプルテンプレートを返します。

指定した宛先 ID が [ベストエフォート集計](./destination-configuration.md#best-effort-aggregation) および `maxUsersPerRequest=1` 集計ポリシーでは、リクエストは次のようなサンプルテンプレートを返します。

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
        {#- Alternative syntax for filtering segments by status: -#}
        {% for segment in removedSegments(input.profile.segmentMembership.ups) %}
            "{{ segment.key }}"{%- if not loop.last -%},{%- endif -%}
        {% endfor %}
        ]
    }
}
```

指定した宛先 ID が、 [設定可能な集計](./destination-configuration.md#configurable-aggregation) または [ベストエフォート集計](./destination-configuration.md#best-effort-aggregation) と `maxUsersPerRequest` 1 より大きい場合、リクエストは次のようなサンプルテンプレートを返します。

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
                {#- Alternative syntax for filtering segments by status: -#}
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

Destination SDKAPI エンドポイントは、一般的なExperience PlatformAPI エラーメッセージの原則に従います。 参照： [API ステータスコード](../../landing/troubleshooting.md#api-status-codes) および [リクエストヘッダーエラー](../../landing/troubleshooting.md#request-header-errors) （Platform トラブルシューティングガイド）を参照してください。

## 次の手順 {#next-steps}

このドキュメントを読んだ後、 `/authoring/testing/template/sample` API エンドポイント。 次に、 [レンダリングテンプレート API エンドポイント](./render-template-api.md) を使用して、テンプレートに基づいて書き出されたプロファイルを生成し、宛先の期待されるデータ形式と比較します。
