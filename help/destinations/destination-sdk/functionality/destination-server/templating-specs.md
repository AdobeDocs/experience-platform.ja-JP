---
description: エンドポイントに送信される HTTP リクエストの書式設定方法を説明します。/authoring/destination-servers エンドポイントを使用して、Adobe Experience Platform Destination SDK の宛先サーバーテンプレート仕様を設定します。
title: Destination SDK で作成される宛先のテンプレート仕様
exl-id: 066781c8-0af0-4958-b62f-194c6ba13f3a
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '523'
ht-degree: 93%

---

# Destination SDK で作成される宛先のテンプレート仕様

宛先サーバー設定のテンプレート仕様部分を使用して、宛先に送信される HTTP リクエストの書式設定方法を設定します。

テンプレート仕様では、XDM スキーマとプラットフォームがサポートする形式の間でのプロファイル属性フィールドの変換方法を定義できます。

テンプレート仕様は、リアルタイム（ストリーミング）宛先用の宛先サーバー設定の一部です。

Destination SDKを使用して作成された統合で、このコンポーネントがどこに適合するかを把握するには、 [設定オプション](../configuration-options.md) ドキュメントを参照するか、 [Destination SDKを使用したストリーミング先の設定](../../guides/configure-destination-instructions.md#create-server-template-configuration).

`/authoring/destination-servers` エンドポイントを介して宛先用のテンプレート仕様を設定できます。このページに表示されるコンポーネントを設定できる、詳細な API 呼び出しの例については、以下の API リファレンスページを参照してください。

* [宛先サーバー設定の作成](../../authoring-api/destination-server/create-destination-server.md)
* [宛先サーバー設定の更新](../../authoring-api/destination-server/update-destination-server.md)

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## サポートされる統合タイプ {#supported-integration-types}

このページで説明される機能をサポートする統合のタイプについて詳しくは、以下の表を参照してください。

| 統合タイプ | 機能のサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベースの（バッチ）統合 | × |

## テンプレート仕様の設定 {#configure-template-spec}

アドビは [Jinja](https://jinja.palletsprojects.com/en/2.11.x/) と類似したテンプレート言語を使用して、XDM スキーマのフィールドを宛先でサポートされる形式に変換します。

![ハイライト表示されたテンプレート設定](../../assets/functionality/destination-server/template-configuration.png)

変換について詳しくは、以下のリンクを参照してください。

* [メッセージ形式](message-format.md)
* [ID、属性およびオーディエンスメンバーシップの変換へのテンプレート言語の使用](message-format.md#using-templating)

>[!TIP]
>
>アドビでは、メッセージ変換テンプレートの作成とテストに役立つ[開発者ツール](../../testing-api/streaming-destinations/create-template.md)を提供しています。

以下の HTTP リクエストテンプレートの例と各パラメーターの説明を参照してください。

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
| `httpMethod` | 文字列 | *必須。* サーバーへの呼び出しでアドビが使用するメソッド。サポートされるメソッド：`GET`、`PUT`、`POST`、`DELETE`、`PATCH`。 |
| `templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `value` | 文字列 | *必須。* この文字列は、Platform によって送信される HTTP リクエストを宛先で想定される形式に書式設定するテンプレートの文字がエスケープされたバージョンです。<br>テンプレートの記述方法について詳しくは、[テンプレートの使用](message-format.md#using-templating)に関する節を参照してください。<br> 文字のエスケープについて詳しくは、[RFC JSON 規格の第 7 節](https://tools.ietf.org/html/rfc8259#section-7)を参照してください。<br>単純な変換の例については、[プロファイル属性](message-format.md#attributes)変換を参照してください。 |
| `contentType` | 文字列 | *必須。* サーバーが受け入れるコンテンツタイプ。変換テンプレートが生成する出力のタイプに応じて、これは、サポートされる任意の [HTTP アプリケーションコンテンツタイプ](https://www.iana.org/assignments/media-types/media-types.xhtml#application)になります。ほとんどの場合、この値は、`application/json` に設定する必要があります。 |

{style="table-layout:auto"}

## 次の手順 {#next-steps}

この記事を読むことで、テンプレート仕様とは何か、およびどのように設定できるかについて、理解を深めることができました。

その他の宛先サーバーコンポーネントについて詳しくは、以下の記事を参照してください。

* [Destination SDK で作成される宛先のサーバー仕様](server-specs.md)
* [メッセージ形式](message-format.md)
* [ファイル形式設定](file-formatting.md)
