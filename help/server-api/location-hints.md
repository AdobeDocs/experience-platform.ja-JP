---
title: 場所のヒント
description: この記事では、エンドユーザーリクエストを常に同じサーバーにルーティングできるように、Edge Network Server API でロケーションヒントがどのように機能するかを説明します。
exl-id: 8cd2f8e2-2065-4b7e-8d35-4ed1a716f1b3
source-git-commit: 80c527ab3c82e01fe19e5003e224d63e79b23bdc
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# 場所のヒント

## 概要 {#overview}

この [!DNL Adobe Experience Platform Edge Network] では、グローバルに分散した複数のサーバを使用して、エンドユーザーの場所に関係なく、迅速な応答時間を確保します。 また、DNS ベースのルーティングを使用して、リクエストが常にエンドユーザーに最も近い Edge ネットワークの場所にルーティングされるようにします。

エンドユーザーがセッション中に VPN に接続したり、モバイルデバイス上のネットワークタイプを切り替えたりした場合、多くの場合、Edge ネットワーク要求は別の場所にルーティングされます。 Adobe Experience PlatformとAdobe Experience Cloudのソリューションはエンドユーザープロファイル情報を Edge ネットワークに保存するので、サーバー間の中間セッションルーティングに問題が生じる可能性があります。

ロケーションのヒントが役立つ場所です。

エンドユーザーが常に現在のプロファイルデータを含む Edge Network 地域サーバーとやり取りできるように、ロケーションヒント機能では、セッションの最初のリクエストがおこなわれたのと同じサーバーに Edge Network へのすべてのリクエストを送信します。 これにより、セッション中にどのようなネットワークの変更が発生したかに関係なく、ユーザーが一貫したエクスペリエンスを得ることができます。

## 場所のヒントの使用方法

次の例に示すように、ロケーションヒントは、最初の Edge Network リクエストの応答と、それ以降のすべてのリクエストに含まれます。

```json
{
   "payload":[
      {
         "scope":"EdgeNetwork",
         "hint":"or2",
         "ttlSeconds":1800
      }
   ],
   "type":"locationHint:result"
}
```

この `EdgeNetwork` 範囲には、Edge ネットワークが後続のリクエストを適宜ルーティングするために必要なすべての関連情報が含まれます。 Edge ネットワークに対する最初のリクエストと後続の各リクエストの応答には、ハンドル内にタイプの `locationHint:result`.

に関連付けられたヒント `EdgeNetwork` 範囲には、次のいずれかの値を含めることができます。

* `or2`
* `va6`
* `irl1`
* `ind1`
* `jpn3`
* `sgp3`
* `aus3`

**API 形式**

後続のリクエストが正しくルーティングされるようにするには、ベースパス間の後続の API 呼び出しの URL パス ( 通常は `ee`、および `v2` API バージョン。

```http
POST 'https://edge.adobedc.net/ee/{LOCATION_HINT}/v2/interact?dataStreamId={DataStream_ID}'
```

## Cookie への場所のヒントの格納 {#storing-hints-in-cookies}

Edge Network が返すロケーションヒントがセッション中も保持されるように、ロケーションヒントの値を、Cookie の有効期間と共に ( `ttlSeconds` フィールド（通常は 1800 秒）に入力します。

ほとんどの Cookie と同様に、Edge ネットワークからの応答があるたびに、この Cookie の有効期間を延長する必要があります。 Web SDK との互換性を最大限に高めるには、cookie 名を使用します。 `kndctr_{IMSORG}_AdobeOrg_cluster`. IMS Org ID は通常、で終わります `@AdobeOrg`. この `@` cookie の形式が正しいことを確認するには、の値をアンダースコアに変換する必要があります。
