---
description: エンドポイントに送信される HTTP リクエストの形式を設定する方法を説明します。 /authoring/destination-servers エンドポイントを使用して、Adobe Experience Platform Destination SDK内の宛先サーバーテンプレート仕様を設定します。
title: Destination SDK
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '524'
ht-degree: 24%

---


# Destination SDK

宛先サーバー設定の template spec 部分を使用して、宛先に送信する HTTP リクエストの形式を設定する方法を設定します。

テンプレート仕様では、XDM スキーマと、プラットフォームがサポートする形式の間でプロファイル属性フィールドを変換する方法を定義できます。

テンプレート仕様は、リアルタイム（ストリーミング）宛先の宛先サーバー設定の一部です。

Destination SDKを使用して作成された統合で、このコンポーネントがどこに適合するかを把握するには、 [設定オプション](../configuration-options.md) ドキュメントを参照するか、 [Destination SDKを使用したストリーミング先の設定](../../guides/configure-destination-instructions.md#create-server-template-configuration).

宛先のテンプレート仕様は、 `/authoring/destination-servers` endpoint. このページに示すコンポーネントを設定できる API 呼び出しの詳細な例については、次の API リファレンスページを参照してください。

* [宛先サーバー設定の作成](../../authoring-api/destination-server/create-destination-server.md)
* [宛先サーバー設定の更新](../../authoring-api/destination-server/update-destination-server.md)

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## サポートされる統合のタイプ {#supported-integration-types}

このページで説明する機能をサポートする統合のタイプについて詳しくは、次の表を参照してください。

| 統合タイプ | 機能をサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベース（バッチ）の統合 | いいえ |

## テンプレート仕様の設定 {#configure-template-spec}

アドビは [Jinja](https://jinja.palletsprojects.com/en/2.11.x/) と類似したテンプレート言語を使用して、XDM スキーマのフィールドを宛先でサポートされる形式に変換します。

![ハイライト表示されたテンプレート設定](../../assets/functionality/destination-server/template-configuration.png)

変換について詳しくは、以下のリンクを参照してください。

* [メッセージの形式](message-format.md)
* [テンプレート言語を使用して ID、属性、セグメントメンバーシップを変換 ](message-format.md#using-templating)

>[!TIP]
>
>アドビは、メッセージ変換テンプレートの作成とテストに役立つ[開発者ツール](../../testing-api/streaming-destinations/create-template.md)を提供しています。

以下に、HTTP リクエストテンプレートの例と、個々のパラメーターの説明を示します。

```json
{
   "httpTemplate":{
      "httpMethod":"POST",
      "requestBody":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{ \"attributes\": [ {% for ns in [\"external_id\", \"yourdestination_id\"] %} {% if input.profile.identityMap[ns] is not empty and first_namespace_encountered %} , {% endif %} {% set first_namespace_encountered = true %} {% for identity in input.profile.identityMap[ns]%} { \"{{ ns }}\": \"{{ identity.id }}\" {% if input.profile.segmentMembership.ups is not empty %} , \"AEPSegments\": { \"add\": [ {% for segment in input.profile.segmentMembership.ups %} {% if segment.value.status == \"realized\" or segment.value.status == \"existing\" %} {% if added_segment_found %} , {% endif %} {% set added_segment_found = true %} \"{{ destination.segmentAliases[segment.key] }}\" {% endif %} {% endfor %} ], \"remove\": [ {% for segment in input.profile.segmentMembership.ups %} {% if segment.value.status == \"exited\" %} {% if removed_segment_found %} , {% endif %} {% set removed_segment_found = true %} \"{{ destination.segmentAliases[segment.key] }}\" {% endif %} {% endfor %} ] } {% set removed_segment_found = false %} {% set added_segment_found = false %} {% endif %} {% if input.profile.attributes is not empty %} , {% endif %} {% for attribute in input.profile.attributes %} \"{{ attribute.key }}\": {% if attribute.value is empty %} null {% else %} \"{{ attribute.value.value }}\" {% endif %} {% if not loop.last%} , {% endif %} {% endfor %} } {% if not loop.last %} , {% endif %} {% endfor %} {% endfor %} ] }"
      },
      "contentType":"application/json"
   }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `httpMethod` | 文字列 | *必須。* サーバーへの呼び出しでアドビが使用するメソッド。サポートされるメソッド： `GET`, `PUT`, `POST`, `DELETE`, `PATCH`. |
| `templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `value` | 文字列 | *必須。* この文字列は、Platform によって送信される HTTP リクエストを、宛先で想定される形式に書式設定する、テンプレートの文字エスケープバージョンです。 <br> テンプレートの書き込み方法について詳しくは、 [テンプレートの使用](message-format.md#using-templating). <br> 文字のエスケープについて詳しくは、[RFC JSON 規格の第 7 節](https://tools.ietf.org/html/rfc8259#section-7)を参照してください。<br> 単純な変換の例については、 [プロファイル属性](message-format.md#attributes) 変換。 |
| `contentType` | 文字列 | *必須。* サーバーが受け入れるコンテンツタイプ。変換テンプレートで生成される出力の種類に応じて、次のいずれかがサポートされます [HTTP アプリケーションコンテンツタイプ](https://www.iana.org/assignments/media-types/media-types.xhtml#application). ほとんどの場合、この値は `application/json`. |

{style="table-layout:auto"}

## 次の手順 {#next-steps}

この記事を読むと、テンプレート仕様の内容と、その設定方法をより深く理解できるはずです。

その他の宛先サーバーコンポーネントの詳細については、次の記事を参照してください。

* [Destination SDK](server-specs.md)
* [メッセージの形式](message-format.md)
* [ファイル形式設定](file-formatting.md)