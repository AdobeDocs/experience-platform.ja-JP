---
description: 宛先テスト API を使用して、宛先を公開する前にストリーミング宛先メッセージ変換テンプレートをテストする方法を説明します。
title: メッセージ変換テンプレートの作成とテスト
exl-id: 15e7f436-4d33-4172-bd14-ad8dfbd5e4a8
source-git-commit: d6402f22ff50963b06c849cf31cc25267ba62bb1
workflow-type: tm+mt
source-wordcount: '950'
ht-degree: 90%

---


# メッセージ変換テンプレートの作成とテスト {#create-template}

## 概要 {#overview}

Destination SDK の一部として、アドビは、宛先の設定およびテストを支援するためのデベロッパーツールを提供しています。このページでは、メッセージ変換テンプレートの作成およびテスト方法を説明します。宛先のテスト方法について詳しくは、[宛先設定のテスト](streaming-destination-testing-overview.md)を参照してください。

Adobe Experience Platform のターゲットスキーマと宛先でサポートされているメッセージ形式の間で&#x200B;**メッセージ変換テンプレートを作成およびテスト**&#x200B;するには、後述する&#x200B;*テンプレートオーサリングツール*&#x200B;を使用します。ソースおよびターゲットスキーマ間のデータ変換について詳しくは、[メッセージ形式ドキュメント](../../functionality/destination-server/message-format.md#using-templating)を参照してください。

以下の図に、メッセージ変換テンプレートの作成およびテストが Destination SDK の[宛先設定ワークフロー](../../guides/configure-destination-instructions.md)にどのように適合するかを示します。

![作成テンプレート手順が宛先設定ワークフローのどこに適合するかを示すグラフィック](../../assets/testing-api/create-template-step.png)

## メッセージ変換テンプレートの作成とテストが必要な理由 {#why-create-message-transformation-template}

Destination SDKでの宛先を作成する最初の手順の 1 つは、オーディエンスのメンバーシップ、ID、プロファイル属性のデータ形式がAdobe Experience Platformから宛先に書き出される際に変換される方法を考えることです。 Adobe XDM スキーマと宛先スキーマとの間の変換について詳しくは、[メッセージ形式ドキュメント](../../functionality/destination-server/message-format.md#using-templating)を参照してください。

変換を成功させるためには、[セグメント、ID およびプロファイル属性を送信するテンプレートの作成](../../functionality/destination-server/message-format.md#segments-identities-attributes)の例に類似した変換テンプレートを提供する必要があります。

アドビは、Adobe XDM 形式から宛先でサポートされている形式にデータを変換するメッセージテンプレートを作成およびテストできる、テンプレートツールを提供しています。ツールには、以下に使用できる 2 つの API エンドポイントがあります。

* *サンプルテンプレート API* を使用して、サンプルテンプレートを取得する。
* *レンダリングテンプレート API* を使用して、サンプルテンプレートをレンダリングする。これにより、結果を宛先で想定されているデータ形式と比較できます。書き出されたデータを宛先で想定されるデータ形式と比較したら、テンプレートを編集できます。この方法で、生成する書き出されたデータを、宛先で想定されるデータ形式に一致させます。

## テンプレート作成前に完了すべき手順 {#prerequisites}

テンプレート作成の準備を整える前に、必ず以下の手順を完了してください。

1. [宛先サーバー設定を作成](../../authoring-api/destination-server/create-destination-server.md)します。生成するテンプレートは、`maxUsersPerRequest` パラメーターに指定した値に基づいて異なります。
   * 用途 `maxUsersPerRequest=1` 宛先への API 呼び出しで単一のプロファイルを、オーディエンス資格、ID、プロファイル属性と共に含める場合。
   * 用途 `maxUsersPerRequest` 宛先への API 呼び出しに複数のプロファイルと、そのオーディエンスの資格、ID、プロファイル属性を含める場合は、値が 1 より大きい。
2. [宛先設定を作成](../../authoring-api/destination-configuration/create-destination-configuration.md)して、宛先サーバー設定の ID を `destinationDelivery.destinationServerId` に追加します。
3. 先ほど作成した[宛先設定の ID を取得](../../authoring-api/destination-configuration/retrieve-destination-configuration.md)して、テンプレート作成ツールで使用します。
4. メッセージ変換テンプレートで[使用できるのはどの関数およびフィルターか](../../functionality/destination-server/supported-functions.md)を理解します。

## サンプルテンプレート API およびレンダリングテンプレート API を使用した、宛先用のテンプレートの作成 {#iterative-process}

>[!TIP]
>
>メッセージ変換テンプレートを作成および編集する前に、まず、任意の変換を適用しない生のプロファイルを書き出すシンプルなテンプレートを使用して[レンダリングテンプレート API エンドポイント](../../testing-api/streaming-destinations/render-template-api.md#render-exported-data)を呼び出すことができます。シンプルなテンプレートの構文を次に示します。<br> `"template": "{% for profile in input.profiles %}{{profile|raw}}{% endfor %}}"`

テンプレートを取得およびテストするプロセスは、反復されます。書き出されたプロファイルが宛先で想定されているデータ形式に一致するまで、以下の手順を繰り返します。

1. 最初に、[サンプルテンプレートを取得](../../testing-api/streaming-destinations/create-template.md#sample-template-api)します。
2. サンプルテンプレートを出発点として使用し、独自のドラフトを作成します。
3. 独自のテンプレートを使用して、[レンダリングテンプレート API エンドポイント](../../testing-api/streaming-destinations/create-template.md#render-template-api)を呼び出します。アドビは、スキーマに基づいてサンプルプロファイルを生成し、結果または発生したエラーを返します。
4. 書き出されたデータを宛先で想定されるデータ形式と比較します。必要に応じて、テンプレートを編集します。
5. 書き出されたプロファイルが宛先で想定されているデータ形式に一致するまで、このプロセスを繰り返します。

## サンプルテンプレート API を使用したサンプルテンプレートの取得 {#sample-template-api}

>[!NOTE]
>
>完全な API リファレンスドキュメントについては、[サンプルテンプレート API 操作の概要](../../testing-api/streaming-destinations/sample-template-api.md)を参照してください。

以下に示すように、呼び出しに宛先 ID を追加すると、応答は、宛先 ID に対応するテンプレートの例を返します。

```shell
curl --location --request GET 'https://platform.adobe.io/data/core/activation/authoring/testing/template/sample/5114d758-ce71-43ba-b53e-e2a91d67b67f' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
```

指定する宛先 ID が、[ベストエフォート集計](../../functionality/destination-configuration/aggregation-policy.md#best-effort-aggregation)と集計ポリシーの `maxUsersPerRequest=1` を含む宛先設定に対応する場合、リクエストは、以下に類似したサンプルテンプレートを返します。

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

## テンプレートの文字をエスケープ {#character-escape-template}

テンプレートを使用して、宛先の想定される形式に一致するプロファイルをレンダリングする前に、以下の画面録画に示すように、テンプレートの文字をエスケープする必要があります。

![オンライン文字エスケープツールを使用してテンプレートの文字をエスケープする方法を示すビデオ](../../assets/testing-api/escape-characters.gif)

オンライン文字エスケープツールを使用できます。上記のデモでは、[JSON Escape formatter](https://jsonformatter.org/json-escape) を使用しています。

## レンダリングテンプレート API {#render-template-api}

[サンプルテンプレート API](create-template.md#sample-template-api) を使用してメッセージ変換テンプレートを作成したら、[テンプレートをレンダリング](render-template-api.md)し、それに基づいて、書き出されたデータを生成できます。これにより、Adobe Experience Platform が宛先に書き出すプロファイルが宛先の想定される形式に一致するかどうかを検証できます。

実行できる呼び出しの例については、API リファレンスを参照してください。

* [本文でプロファイルを送信しないテンプレートをレンダリング](render-template-api.md#multiple-profiles-no-body)
* [本文でプロファイルを送信するテンプレートをレンダリング](render-template-api.md#multiple-profiles-with-body)

書き出されたプロファイルが宛先で想定されているデータ形式に一致するまで、テンプレートを編集して、レンダリングテンプレート API エンドポイントへの呼び出しを行います。

## 文字がエスケープされたテンプレートの宛先サーバー設定への追加

メッセージ変換テンプレートに満足したら、[宛先サーバー設定](../../authoring-api/destination-server/create-destination-server.md)の `httpTemplate.requestBody.value` に追加します。
