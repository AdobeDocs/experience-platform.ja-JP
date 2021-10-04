---
description: 宛先 SDK の一部として、Adobeには、宛先の設定とテストを支援する開発者ツールが用意されています。 このページでは、メッセージ変換テンプレートの作成およびテスト方法について説明します。
title: メッセージ変換テンプレートの作成とテスト
exl-id: 15e7f436-4d33-4172-bd14-ad8dfbd5e4a8
source-git-commit: 2ed132cd16db64b5921c5632445956f750fead56
workflow-type: tm+mt
source-wordcount: '909'
ht-degree: 0%

---

# メッセージ変換テンプレートの作成とテスト {#create-template}

## 概要 {#overview}

宛先 SDK の一部として、Adobeには、宛先の設定とテストを支援する開発者ツールが用意されています。 このページでは、メッセージ変換テンプレートの作成およびテスト方法について説明します。 宛先のテスト方法について詳しくは、[ 宛先設定のテスト ](./test-destination.md) を参照してください。

**Adobe Experience Platformのターゲットスキーマと、宛先でサポートされるメッセージ形式との間にメッセージ変換テンプレート** を作成してテストするには、後述の *テンプレートオーサリングツール* を使用します。  ソースとターゲットのスキーマ間でのデータ変換について詳しくは、[ メッセージ形式のドキュメント ](./message-format.md#using-templating) を参照してください。

次の図に、宛先 SDK の [ 宛先設定ワークフロー ](./configure-destination-instructions.md) に適合するメッセージ変換テンプレートの作成とテスト方法を示します。

![テンプレートの作成手順が宛先設定ワークフローに適合する場所のグラフィック](./assets/create-template-step.png)

## メッセージ変換テンプレートの作成とテストが必要な理由 {#why-create-message-transformation-template}

宛先 SDK での宛先を作成する最初の手順の 1 つは、セグメントのメンバーシップ、ID、プロファイル属性のデータ形式が、Adobe Experience Platformから宛先に書き出される際にどのように変換されるかを考えることです。 AdobeXDM スキーマと宛先スキーマの変換に関する情報を、[ メッセージ形式のドキュメント ](./message-format.md#using-templating) で確認します。

変換を正常に実行するには、次の例のような変換テンプレートを指定する必要があります。[ セグメント、ID およびプロファイル属性を送信するテンプレートを作成します ](./message-format.md#segments-identities-attributes)。

Adobeは、AdobeXDM 形式のデータを、宛先でサポートされる形式に変換するメッセージテンプレートを作成およびテストできるテンプレートツールを提供します。 このツールには、次の 2 つの API エンドポイントを使用できます。
* *サンプルテンプレート API* を使用して、サンプルテンプレートを取得します。
* *レンダリングテンプレート API* を使用してサンプルテンプレートをレンダリングし、結果と宛先の予想されるデータ形式を比較できます。 書き出されたデータを、宛先で予想されるデータ形式と比較した後、テンプレートを編集できます。 これにより、書き出されたデータは、宛先で予想されるデータ形式と一致します。

## サンプルテンプレート API とレンダリングテンプレート API を使用して、宛先のテンプレートを作成する方法 {#iterative-process}

テンプレートを取得してテストするプロセスは反復的です。 書き出されたプロファイルが宛先の期待されるデータ形式と一致するまで、以下の手順を繰り返します。

1. まず、[ サンプルテンプレート ](./create-template.md#sample-template-api) を取得します。
2. サンプルテンプレートを基に、独自のドラフトを作成します。
3. 独自のテンプレートで [ レンダリングテンプレート API エンドポイント ](./create-template.md#render-template-api) を呼び出します。 Adobeは、スキーマに基づいてサンプルプロファイルを生成し、結果または発生したエラーを返します。
4. 書き出されたデータと、宛先で予想されるデータ形式を比較します。 必要に応じて、テンプレートを編集します。
5. 書き出されたプロファイルが宛先の期待されるデータ形式と一致するまで、このプロセスを繰り返します。

## テンプレートを作成する前に完了する手順 {#prerequisites}

テンプレートを作成する準備が整う前に、次の手順を実行してください。

1. [宛先サーバー設定の作成](./destination-server-api.md)を参照してください。生成するテンプレートは、`maxUsersPerRequest` パラメーターに指定した値に基づいて異なります。
   * 宛先への API 呼び出しに単一のプロファイルと、そのセグメント認定、ID、プロファイル属性を含める場合は、 `maxUsersPerRequest=1` を使用します。
   * 宛先への API 呼び出しに複数のプロファイルと、そのセグメント認定、ID、プロファイル属性を含める場合は、 `maxUsersPerRequest` を 1 より大きい値で使用します。
2. [宛先設定を作](./destination-configuration-api.md#create) 成し、で宛先サーバー設定の ID を追加しま `destinationDelivery.destinationServerId`す。
3. [作成した宛先設定の ID](./destination-configuration-api.md#retrieve-list) を取得し、テンプレート作成ツールで使用できるようにします。

## サンプルテンプレート API を使用したサンプルテンプレートの取得 {#sample-template-api}

>[!NOTE]
>
>API リファレンスに関する完全なドキュメントについては、[Get sample template API operations](./sample-template-api.md) を参照してください。

以下に示すように、呼び出しに宛先 ID を追加すると、応答は宛先 ID に対応するテンプレートの例を返します。

```shell
curl --location --request GET 'https://platform.adobe.io/data/core/activation/authoring/testing/template/sample/5114d758-ce71-43ba-b53e-e2a91d67b67f' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
```

指定した宛先 ID が、集計ポリシーの [ ベストエフォートの集計 ](./destination-configuration.md#best-effort-aggregation) と `maxUsersPerRequest=1` を使用した宛先設定に対応している場合、リクエストは次のようなサンプルテンプレートを返します。

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

指定した宛先 ID が、[ 構成可能な集計 ](./destination-configuration.md#configurable-aggregation) または [ ベストエフォート集計 ](./destination-configuration.md#best-effort-aggregation) が 1 より大きい宛先サーバーテンプレートに対応する場合、リクエストは次のようなサンプルテンプレートを返します。`maxUsersPerRequest`

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

## テンプレートの文字エスケープ {#character-escape-template}

テンプレートを使用して宛先の想定される形式に一致するプロファイルをレンダリングする前に、テンプレートを文字エスケープする必要があります（下の画面の記録を参照）。

![オンラインの文字エスケープツールを使用してテンプレートを文字エスケープする方法を示すビデオ](./assets/escape-characters.gif)

オンラインの文字エスケープツールを使用できます。 上記のデモでは、[JSON エスケープフォーマッター ](https://jsonformatter.org/json-escape) を使用しています。

## レンダリングテンプレート API {#render-template-api}

[ サンプルテンプレート API](./create-template.md#sample-template-api) を使用してメッセージ変換テンプレートを作成したら、[ テンプレートをレンダリングして ](./render-template-api.md)、テンプレートに基づいて書き出されたデータを生成できます。 これにより、Adobe Experience Platformが宛先に書き出すプロファイルが、目的の宛先の形式と一致するかどうかを確認できます。

実行できる呼び出しの例については、 API リファレンスを参照してください。

* [本文でプロファイルを送信しないテンプレートのレンダリング](./render-template-api.md#multiple-profiles-no-body)
* [本文で送信したプロファイルを使用したテンプレートのレンダリング](./render-template-api.md#multiple-profiles-with-body)

テンプレートを編集し、書き出されたプロファイルが宛先の予想されるデータ形式と一致するまで、レンダーテンプレート API エンドポイントを呼び出します。

## 文字エスケープテンプレートを宛先サーバーの設定に追加する

メッセージ変換テンプレートの設定が完了したら、 `httpTemplate.requestBody.value` の [ 宛先サーバー設定 ](./server-and-template-configuration.md) に追加します。
