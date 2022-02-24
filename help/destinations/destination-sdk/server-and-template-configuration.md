---
description: サーバーおよびテンプレートの仕様は、共通のエンドポイント「/authoring/destination-servers」を介してAdobe Experience Platform Destination SDKで設定できます。
title: Destination SDKのサーバーおよびテンプレート仕様の構成オプション
exl-id: cf493ed5-0bdb-4b90-b84d-73926a566a2a
source-git-commit: 92bca3600d854540fd2badd925e453fba41601a7
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 8%

---

# ストリーミング宛先サーバーおよびテンプレート仕様の設定オプション

## 概要 {#overview}

サーバーおよびテンプレートの仕様は、共通のエンドポイントを介してAdobe Experience Platform Destination SDKで設定できます `/authoring/destination-servers`. 読み取り [宛先 API エンドポイントの操作](./destination-server-api.md) エンドポイントで実行できる操作の完全なリストについては、を参照してください。

## サーバーの仕様 {#server-specs}

![強調表示されたサーバー設定](./assets/server-configuration.png)

お客様は、HTTP エクスポートを通じて、Adobe Experience Platformから宛先へとデータをアクティブ化できます。 サーバー設定には、メッセージを受信するサーバー（ユーザー側のサーバー）に関する情報が含まれています。

このプロセスは、ユーザーデータを一連の HTTP メッセージとして宛先プラットフォームに配信します。 以下のパラメーターは、 HTTP サーバー仕様テンプレートを構成します。

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | *必須* サーバーのわかりやすい名前を表し、Adobeのみに表示されます。 この名前は、パートナーや顧客には表示されません。 例 `Moviestar destination server`. |
| `destinationServerType` | 文字列 | *必須* に設定 `URL_BASED` ストリーミング先用 |
| `templatingStrategy` | 文字列 | *必須.* <ul><li>用途 `PEBBLE_V1` 固定値の代わりにマクロを使用する場合は、 `value` フィールドに入力します。 次のようなエンドポイントがある場合は、このオプションを使用します。 `https://api.moviestar.com/data/{{customerData.region}}/items` </li><li> 用途 `NONE` Adobe側で変換が必要ない場合（例えば、次のようなエンドポイントがある場合）、 `https://api.moviestar.com/data/items` </li></ul> |
| `value` | 文字列 | *必須* Experience Platformが接続する API エンドポイントのアドレスを入力します。 |

{style=&quot;table-layout:auto&quot;}

## テンプレート仕様 {#template-specs}

![強調表示されたテンプレート設定](./assets/template-configuration.png)

テンプレート仕様を使用すると、書き出したメッセージを宛先にフォーマットする方法を設定できます。 Adobeでは、 [神社](https://jinja.palletsprojects.com/en/2.11.x/) を使用して、XDM スキーマのフィールドを、宛先でサポートされている形式に変換します。 変換について詳しくは、以下のリンクを参照してください。

* [メッセージのフォーマット](./message-format.md)
* [ID、属性、セグメントメンバーシップの変換にテンプレート言語を使用する ](./message-format.md#using-templating)

>[!TIP]
>
>Adobeオファー [開発者ツール](./create-template.md) これは、メッセージ変換テンプレートの作成とテストに役立ちます。

## ストリーミング宛先のサンプル設定  {#example-configuration}

```json
{
   "name":"Moviestar destination server",
   "destinationServerType":"URL_BASED",
   "urlBasedDestination":{
      "url":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"https://api.moviestar.com/data/{{customerData.region}}/items"
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
| `httpMethod` | 文字列 | *必須* サーバーへの呼び出しでAdobeが使用するメソッド。 オプションは次のとおりです。 `GET`, `PUT`, `POST`, `DELETE`, `PATCH`. |
| `templatingStrategy` | 文字列 | *必須* 用途 `PEBBLE_V1`. |
| `value` | 文字列 | *必須* この文字列は、Platform の顧客のデータを、サービスが想定する形式に変換する、文字エスケープバージョンです。 <br> テンプレートの書き込み方法について詳しくは、 [テンプレートセクションの使用](./message-format.md#using-templating). <br> 文字のエスケープについて詳しくは、 [RFC JSON 標準、セクション 7](https://tools.ietf.org/html/rfc8259#section-7). <br> 単純な変換の例については、 [プロファイル属性](./message-format.md#attributes) 変換。 |
| `contentType` | 文字列 | *必須* サーバーが受け入れるコンテンツタイプ。 この値は最も可能性が高い `application/json`. |

{style=&quot;table-layout:auto&quot;}