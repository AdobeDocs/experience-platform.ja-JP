---
description: Adobeは、Destination SDKの一環として、宛先の設定とテストを支援する開発者ツールを提供します。 ここでは、メッセージ変換テンプレートの作成およびテスト方法について説明します。
title: メッセージ変換テンプレートの作成とテスト
exl-id: 15e7f436-4d33-4172-bd14-ad8dfbd5e4a8
source-git-commit: 97ffaa2a53dbbf5a7be5f002e63be4ed3339f565
workflow-type: tm+mt
source-wordcount: '960'
ht-degree: 2%

---

# メッセージ変換テンプレートの作成とテスト {#create-template}

## 概要 {#overview}

Adobeは、Destination SDKの一環として、宛先の設定とテストを支援する開発者ツールを提供します。 ここでは、メッセージ変換テンプレートの作成およびテスト方法について説明します。 宛先のテスト方法について詳しくは、 [宛先設定のテスト](./test-destination.md).

宛先 **メッセージ変換テンプレートの作成とテスト** Adobe Experience Platformのターゲットスキーマと、宛先でサポートされるメッセージ形式の間で、 *テンプレートオーサリングツール* 以下で詳しく説明します。  詳しくは、 [メッセージ形式ドキュメント](./message-format.md#using-templating).

次の図に、メッセージ変換テンプレートが [宛先設定ワークフロー](./configure-destination-instructions.md) Destination SDK:

![テンプレート作成手順が宛先設定ワークフローに適用される場所のグラフィック](./assets/create-template-step.png)

## メッセージ変換テンプレートの作成とテストが必要な理由 {#why-create-message-transformation-template}

Destination SDKでの宛先を作成する最初の手順の 1 つは、セグメントメンバーシップ、ID、プロファイル属性のデータ形式がAdobe Experience Platformから宛先に書き出される際にどのように変換されるかを考えることです。 で宛先スキーマ XDMAdobeとの変換に関する情報を見つけます。 [メッセージ形式ドキュメント](./message-format.md#using-templating).

変換を正常に実行するには、次の例のような変換テンプレートを指定する必要があります。 [セグメント、ID およびプロファイル属性を送信するテンプレートの作成](./message-format.md#segments-identities-attributes).

Adobeは、AdobeXDM 形式のデータを、宛先でサポートされる形式に変換するメッセージテンプレートを作成およびテストできるテンプレートツールを提供します。 このツールには、次の 2 つの API エンドポイントを使用できます。
* 以下を使用： *サンプルテンプレート API* をクリックして、サンプルテンプレートを取得します。
* 以下を使用： *レンダリングテンプレート API* を使用してサンプルテンプレートをレンダリングし、結果を宛先の期待されるデータ形式と比較できるようにします。 書き出されたデータを、宛先で想定されるデータフォーマットと比較した後、テンプレートを編集できます。 この方法では、生成した書き出しデータは、宛先で想定されているデータ形式と一致します。

## テンプレートを作成する前に完了する手順 {#prerequisites}

テンプレートを作成する準備が整う前に、次の手順を実行してください。

1. [宛先サーバー設定の作成](./destination-server-api.md). 生成するテンプレートは、 `maxUsersPerRequest` パラメーター。
   * 用途 `maxUsersPerRequest=1` 宛先への API 呼び出しに単一のプロファイルを含める場合は、そのセグメント認定、ID、プロファイル属性を含めます。
   * 用途 `maxUsersPerRequest` 宛先への API 呼び出しに複数のプロファイルと、そのセグメント認定、ID、プロファイル属性を含める場合は、値が 1 より大きい。
2. [宛先設定の作成](./destination-configuration-api.md#create) をクリックし、 `destinationDelivery.destinationServerId`.
3. [宛先設定の ID を取得する](./destination-configuration-api.md#retrieve-list) 作成したばかりのテンプレートを、テンプレート作成ツールで使用できます。
4. 理解 [使用できる関数とフィルター](./supported-functions.md) 」と入力します。

## サンプルテンプレート API とレンダリングテンプレート API を使用して、宛先のテンプレートを作成する方法 {#iterative-process}

>[!TIP]
>
>メッセージ変換テンプレートを作成および編集する前に、まず [レンダリングテンプレート API エンドポイント](./render-template-api.md#render-exported-data) 変換を適用せずに生のプロファイルを書き出す単純なテンプレートを使用します。 シンプルなテンプレートの構文は次のとおりです。<br> `"template": "{% for profile in input.profiles %}{{profile|raw}}{% endfor %}}"`

テンプレートを取得してテストするプロセスは反復的です。 書き出されたプロファイルが宛先の想定されるデータ形式に一致するまで、以下の手順を繰り返します。

1. まず、 [サンプルテンプレートの取得](./create-template.md#sample-template-api).
2. サンプルテンプレートを基に、独自のドラフトを作成します。
3. を [レンダリングテンプレート API エンドポイント](./create-template.md#render-template-api) 独自のテンプレートを使用して、 Adobeは、スキーマに基づいてサンプルプロファイルを生成し、結果または発生したエラーを返します。
4. 書き出されたデータを、宛先で予想されるデータフォーマットと比較します。 必要に応じて、テンプレートを編集します。
5. 書き出されたプロファイルが宛先の想定されるデータ形式に一致するまで、この処理を繰り返します。

## サンプルテンプレート API を使用したサンプルテンプレートの取得 {#sample-template-api}

>[!NOTE]
>
>API リファレンスのドキュメントについて詳しくは、 [「Get sample template API」操作](./sample-template-api.md).

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

## テンプレートを文字エスケープ {#character-escape-template}

テンプレートを使用して宛先の想定される形式に一致するプロファイルをレンダリングする前に、テンプレートを文字エスケープする必要があります（下の画面での記録を参照）。

![オンラインの文字エスケープツールを使用してテンプレートを文字エスケープする方法を示すビデオ](./assets/escape-characters.gif)

オンラインの文字エスケープツールを使用できます。 上記のデモでは、 [JSON Escape フォーマッター](https://jsonformatter.org/json-escape).

## レンダリングテンプレート API {#render-template-api}

メッセージ変換テンプレートを作成した後、 [サンプルテンプレート API](./create-template.md#sample-template-api)、 [テンプレートをレンダリング](./render-template-api.md) をクリックして、書き出されたデータを生成します。 これにより、Adobe Experience Platformが宛先に書き出すプロファイルが、目的の宛先の想定される形式と一致しているかどうかを確認できます。

実行できる呼び出しの例については、 API リファレンスを参照してください。

* [本文に送信されたプロファイルを含まないテンプレートのレンダリング](./render-template-api.md#multiple-profiles-no-body)
* [本文で送信されたプロファイルを使用してテンプレートをレンダリングする](./render-template-api.md#multiple-profiles-with-body)

テンプレートを編集し、書き出されたプロファイルが宛先の想定されるデータ形式に一致するまで、レンダリングテンプレート API エンドポイントを呼び出します。

## 文字エスケープテンプレートを宛先サーバー設定に追加する

メッセージの変換テンプレートの設定が完了したら、そのテンプレートを [宛先サーバーの設定](./server-and-template-configuration.md)、 `httpTemplate.requestBody.value`.
