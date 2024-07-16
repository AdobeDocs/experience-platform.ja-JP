---
title: 場所のヒント
description: この記事では、Edge Networkサーバー API で場所のヒントが機能し、エンドユーザーのリクエストを常に同じサーバーにルーティングできるようにする方法について説明します。
exl-id: 8cd2f8e2-2065-4b7e-8d35-4ed1a716f1b3
source-git-commit: 2c7a5f007189d897ed32302a2a80c1e16af6af80
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 1%

---

# 場所のヒント

## 概要 {#overview}

この [!DNL Adobe Experience Platform Edge Network] は、エンドユーザーの場所に関係なく、高速な応答時間を確保するために、複数のグローバルに分散したサーバーを使用します。 また、DNS ベースのルーティングを使用して、リクエストが常にエンドユーザーに最も近いEdge Networkの場所にルーティングされるようにします。

エンドユーザーがセッションの間に VPN に接続したり、モバイルデバイス上のネットワークタイプを切り替えたりすると、Edge Networkリクエストが別の場所にルーティングされることがよくあります。 Adobe Experience PlatformおよびAdobe Experience Cloud ソリューションでは、エンドユーザーのプロファイル情報がサーバー上に保存されるので、Edge Network間のミッドセッションルーティングは問題を引き起こす可能性があります。

場所のヒントは、ここで役に立ちます。

エンドユーザーが現在のプロファイルデータを含むEdge Network地域サーバーを常に確実に操作できるように、場所ヒント機能は、セッションへのすべてのリクエストが、Edge Networkの最初のリクエストが行われたのと同じサーバーに送信されるようにします。 これにより、ユーザーは、セッション中にどのようなネットワーク変更が発生しても、一貫したエクスペリエンスを得ることができます。

## 場所のヒントの使用

次の例に示すように、場所のヒントは、最初のEdge Networkリクエストの応答と、以降のすべてのリクエストに含まれます。

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

`EdgeNetwork` スコープには、Edge Networkが後続のリクエストを適切にルーティングする必要があるすべての関連情報が含まれています。 Edge ネットワークへの最初のリクエストとそれに続く各リクエストの応答では、ハンドルに `locationHint:result` のタイプのセクションが存在します。

`EdgeNetwork` 範囲に関連付けられているヒントには、次のいずれかの値を含めることができます。

* `or2`
* `va6`
* `irl1`
* `ind1`
* `jpn3`
* `sgp3`
* `aus3`

**API 形式**

後続のリクエストが正しくルーティングされるようにするには、ベースパス（通常は `ee` と API バージョンの間の後続の API 呼び出しの URL パスに場所のヒント `v2` 挿入します。

```http
POST 'https://edge.adobedc.net/ee/{LOCATION_HINT}/v2/interact?dataStreamId={DataStream_ID}'
```

## Cookie への場所のヒントの保存 {#storing-hints-in-cookies}

Edge Networkによって返される場所のヒントがセッション中に保持されるようにするには、場所のヒント値を cookie に格納し、`ttlSeconds` フィールドに含まれる cookie の有効期間（通常は 1800 秒）を格納できます。

ほとんどの Cookie と同様に、Edge Networkからの応答があるたびに、この Cookie の有効期間を延長する必要があります。 Web SDK との互換性を最大限に高めるには、cookie 名 `kndctr_{IMSORG}_AdobeOrg_cluster` を使用します。 組織 ID は通常、`@AdobeOrg` で終わります。 cookie の形式を正しくするには、`@` の値をアンダースコアに変換する必要があります。
