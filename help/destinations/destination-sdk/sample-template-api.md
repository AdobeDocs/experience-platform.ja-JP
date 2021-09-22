---
description: このページでは、「/authoring/testing/template/sample」 APIエンドポイントを使用して実行できる、宛先のテストメッセージ変換テンプレートを取得するためのAPI操作の一覧と説明を示します。
title: 「Get sample template API」操作
exl-id: d18a06f7-0c3a-4b4d-a7d5-011690d00e2c
source-git-commit: 2ed132cd16db64b5921c5632445956f750fead56
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 2%

---

# サンプルテンプレートAPI操作の取得{#get=sample-template-api-operations}

>[!IMPORTANT]
>
>**API エンドポイント**: `https://platform.adobe.io/data/core/activation/authoring/testing/template/sample`

このページでは、`/authoring/testing/template/sample` APIエンドポイントを使用して、宛先の[メッセージ変換テンプレート](./message-format.md#using-templating)を生成するために実行できるすべてのAPI操作について説明します。 このエンドポイントでサポートされる機能の説明については、[テンプレートの作成](./create-template.md)を参照してください。

## サンプルテンプレートAPI操作の概要 {#get-started}

続行する前に、[はじめに](./getting-started.md)を参照し、必要な宛先オーサリング権限や必要なヘッダーの取得方法など、APIを正しく呼び出すために必要な重要な情報を確認してください。

## サンプルテンプレートの取得 {#generate-sample-template}

`authoring/testing/template/sample/`エンドポイントにGETリクエストを送信し、テンプレートを作成する場所に基づいて宛先設定の宛先IDを指定することで、サンプルテンプレートを取得できます。

>[!TIP]
>
>* ここで使用する宛先IDは、`/destinations`エンドポイントを使用して作成された、宛先設定に対応する`instanceId`です。 [宛先設定APIリファレンス](./destination-configuration-api.md#retrieve-list)を参照してください。


**API 形式**


```http
GET authoring/testing/template/sample/{DESTINATION_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{DESTINATION_ID}` | メッセージ変換テンプレートを生成する宛先設定のID。 |

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

正常な応答は、HTTPステータス200と、期待されるデータ形式に合わせて編集できるサンプルテンプレートを返します。

指定した宛先IDが、集計ポリシーの[best effort aggregation](./destination-configuration.md#best-effort-aggregation)と`maxUsersPerRequest=1`を使用した宛先設定に対応している場合、リクエストは次のようなサンプルテンプレートを返します。

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

指定した宛先IDが、[構成可能な集計](./destination-configuration.md#configurable-aggregation)または[ベストエフォート集計](./destination-configuration.md#best-effort-aggregation)が1より大きい宛先サーバーテンプレートに対応する場合、リクエストは次のようなサンプルテンプレートを返します。`maxUsersPerRequest`

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

## APIエラー処理 {#api-error-handling}

宛先SDK APIエンドポイントは、一般的なExperience PlatformAPIエラーメッセージの原則に従います。 Platformトラブルシューティングガイドの[APIステータスコード](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#api-status-codes)および[リクエストヘッダーエラー](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#request-header-errors)を参照してください。

## 次の手順 {#next-steps}

このドキュメントを読むと、 `/authoring/testing/template/sample` APIエンドポイントを使用してメッセージ変換テンプレートを生成する方法がわかります。 次に、 [レンダリングテンプレートAPIエンドポイント](./render-template-api.md)を使用して、テンプレートに基づいて書き出されたプロファイルを生成し、宛先で予想されるデータ形式と比較します。
