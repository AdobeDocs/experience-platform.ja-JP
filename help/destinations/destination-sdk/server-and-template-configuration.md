---
description: サーバーとテンプレートの仕様は、共通のエンドポイント /authoring/destination-servers を介して Adobe Experience Platform Destination SDK で設定できます。
title: Destination SDK のサーバーとテンプレートの仕様の設定オプション
exl-id: cf493ed5-0bdb-4b90-b84d-73926a566a2a
source-git-commit: a08201c4bc71b0e37202133836e9347ed4d3cd6b
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 100%

---

# ストリーミング宛先のサーバーとテンプレートの仕様の設定オプション

## 概要 {#overview}

サーバーとテンプレートの仕様は、共通のエンドポイント `/authoring/destination-servers` を介して Adobe Experience Platform Destination SDK で設定できます。エンドポイントで実行できる操作の完全なリストについては、[宛先 API エンドポイントの操作](./destination-server-api.md)を参照してください。

## サーバーの仕様 {#server-specs}

![ハイライト表示されたサーバー設定](./assets/server-configuration.png)

お客様は、HTTP エクスポートを介して、Adobe Experience Platform から宛先へのデータを有効化できます。サーバー設定には、メッセージを受信するサーバー（お客様側のサーバー）に関する情報が含まれています。

このプロセスは、ユーザーデータを一連の HTTP メッセージとして宛先プラットフォームに配信します。HTTP サーバー仕様のテンプレートとなるパラメーターは次のとおりです。

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | *必須。* サーバーのわかりやすい名前を表し、アドビにのみ表示されます。この名前は、パートナーや顧客には表示されません。例えば `Moviestar destination server` です。 |
| `destinationServerType` | 文字列 | *必須。* ストリーミング宛先の場合は `URL_BASED` に設定します。 |
| `templatingStrategy` | 文字列 | *必須。* <ul><li>`value` フィールドで固定値ではなくマクロを使用している場合は、`PEBBLE_V1` を使用します。`https://api.moviestar.com/data/{{customerData.region}}/items` のようなエンドポイントがある場合は、このオプションを使用します。 </li><li> アドビ側での変換が不要な場合（例えば、`https://api.moviestar.com/data/items` のようなエンドポイントがある場合）は、`NONE` を使用します。 </li></ul> |
| `value` | 文字列 | *必須。* Experience Platform が接続する API エンドポイントのアドレスを入力します。 |

{style=&quot;table-layout:auto&quot;}

## テンプレート仕様 {#template-specs}

![ハイライト表示されたテンプレート設定](./assets/template-configuration.png)

テンプレート仕様を使用すると、宛先に書き出したメッセージをフォーマットする方法を設定できます。アドビは [Jinja](https://jinja.palletsprojects.com/en/2.11.x/) と類似したテンプレート言語を使用して、XDM スキーマのフィールドを宛先でサポートされる形式に変換します。変換について詳しくは、以下のリンクを参照してください。

* [メッセージの形式](./message-format.md)
* [テンプレート言語を使用して ID、属性、セグメントメンバーシップを変換 ](./message-format.md#using-templating)

>[!TIP]
>
>アドビは、メッセージ変換テンプレートの作成とテストに役立つ[開発者ツール](./create-template.md)を提供しています。

## ストリーミング宛先のサンプル設定  {#example-configuration}

```json
{
   "name":"Moviestar destination server",
   "destinationServerType":"URL_BASED",
   "urlBasedDestination":{
      "url":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"https://api.moviestar.com/data/{{customerData.endpointRegion}}/items"
      }
   },
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
| `httpMethod` | 文字列 | *必須。* サーバーへの呼び出しでアドビが使用するメソッド。オプションは、`GET`、`PUT`、`POST`、`DELETE`、`PATCH` です。 |
| `templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `value` | 文字列 | *必須。* この文字列は、Platform 顧客のデータを、ご利用のサービスが期待する形式に変換する、文字エスケープバージョンです。<br> テンプレートの書き込み方法について詳しくは、[テンプレートセクションの使用](./message-format.md#using-templating)を参照してください。<br> 文字のエスケープについて詳しくは、[RFC JSON 規格の第 7 節](https://tools.ietf.org/html/rfc8259#section-7)を参照してください。<br> 単純な変換の例については、[プロファイル属性](./message-format.md#attributes)変換を参照してください。 |
| `contentType` | 文字列 | *必須。* サーバーが受け入れるコンテンツタイプ。この値は `application/json` である可能性が高いです。 |

{style=&quot;table-layout:auto&quot;}