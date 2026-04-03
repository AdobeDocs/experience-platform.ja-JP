---
title: idMigrationEnabled
description: Web SDKがAMCV Cookieを読み取れるようにします。
exl-id: 33b9d645-0fbe-4fe4-8847-e6f9e78557b6
source-git-commit: bf0bb72777cacd822fd6e887ac3ef71764784214
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 0%

---

# `idMigrationEnabled`

`idMigrationEnabled` プロパティを使用すると、Web SDKは以前のAdobe Experience Cloudの実装で設定されたAMCV Cookieを読み取ることができます。 組織が実装をWeb SDKにアップグレードする場合、この設定を使用すると、現在のAdobe Experience Cloud ID サービスにスムーズに移行できます。 この設定は、Web SDKにアップグレードする際に、一意の訪問者が急激に増加しないように役立ちます。

組織で新しいWeb SDKの実装を実行している場合、この設定を有効にしてもデータの収集や訪問者の識別には影響しません。 すべての実装でイネーブルにしておくことには欠点はありません。

## ID移行の仕組み {#how-it-works}

Visitor APIは、Experience Cloud ID （ECID）を`AMCV_<ORG_ID>`というCookieに保存します。 `idMigrationEnabled`が`true` （デフォルト）の場合、Web SDKはAMCV CookieからECIDを自動的に読み取り、最初のEdge Network リクエストで訪問者のIDとして使用します。

1. 訪問者は、Visitor APIの代わりにWeb SDKを使用するようになったページにアクセスします。
1. Web SDKは、既存の`kndctr_` ID Cookie （独自のCookie フォーマット）をチェックします。 何も見つからない場合は、`AMCV_` Cookieを探します。
1. `AMCV_` Cookieが見つかった場合、Web SDKはECIDを抽出し、それを使用して訪問者のIDを初期化します。
1. Web SDKは、同じECIDを持つ新しい`kndctr_` ID Cookieを設定します。
1. その後の訪問時に、Web SDKは`kndctr_` Cookieを直接使用します。 `AMCV_` Cookieは不要になりました。

移行を機能させるには、Web SDKは、Visitor APIが使用したのと同じ[`orgId`](/help/collection/js/commands/configure/orgid.md)で設定し、AMCV Cookieが設定された同じドメイン（または同じ親ドメインのサブドメイン）にデプロイする必要があります。

## サポートされている移行シナリオ

ID移行では、次の移行パターンがサポートされています。

* **一部のページでは引き続きVisitor APIを使用しますが、他のページではWeb SDKを使用します**: SDKは既存のAMCV Cookieを読み取り、既存のECIDで新しいCookieを書き込みます。 また、まだVisitor APIを使用しているページが同じ訪問者を引き続き認識できるように、AMCV Cookieも書き込みます。
* **Web SDKとVisitor APIはどちらも同じページに存在します**: AMCV Cookieが設定されていない場合、SDKはページ上のVisitor APIを検索し、それを使用してECIDを取得します。
* **サイトはWeb SDKに完全に移行されました**：移行を一定期間有効のままにしておくと、既存のAMCV ベースの訪問者がCookieの移行中も継続性を維持できます。

>[!IMPORTANT]
>
>AMCV Cookieが既に設定されていることを確認していない限り、Visitor APIとWeb SDKを同じページで同時に読み込まないでください。 1つのページで両方のライブラリを実行すると、各ライブラリが個別にIDを管理しようとする競合状態が発生し、重複または競合するECIDが発生する可能性があります。

## 移行をオフにする {#turn-off-migration}

サイト全体がWeb SDKで実行され、対応する`AMCV_` Cookieのない`kndctr_` Cookieのみを持つ訪問者がいない場合、`idMigrationEnabled`を`false`に設定すると、IDの移行を安全に無効にできます。 これはパフォーマンスのマイナーな最適化であり、ID ロジックの表面積を減らします。

ガイドラインとして、最後のページの移行後にAMCV Cookieの最大有効期間が経過するまで待ちます。 AMCV Cookieの有効期限が2年の場合は、最後のページがWeb SDKに切り取られてから2年待ちます。 実際には、ほとんどの組織は（数ヶ月後に）早く移行を無効にし、長い不在の後に初めて戻る少数の訪問者から無視できない量のインフレを受け入れます。

## Audience Manager特性の更新

XDM形式のデータが移行中にAudience Managerに送信される場合、そのデータを信号に変換する必要があります。 XDMが提供する新しいキーを反映するために、特性を更新する必要があります。 [BAAAM ツール &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/bulk-management-tools/bulk-management-intro.html#getting-started-with-bulk-management)を使用すると、このプロセスが簡単になります。

## サードパーティ IDの移行 {#third-party-id}

Visitor APIの実装でサードパーティ ID （`demdex.net` Cookie）が使用されている場合、Web SDKでも移行できます。 Web SDK設定で[`thirdPartyCookiesEnabled`](/help/collection/js/commands/configure/thirdpartycookiesenabled.md)から`true`を設定します。 Web SDKは、既存のDemdex cookieを読み取り、AMCV cookieと同じ移行パターンに従って、Edge Network リクエストにサードパーティ IDを含めます。

## 検証 {#validation}

IDの移行が正しく機能していることを確認するには：

1. 既存の`AMCV_` Cookieを持つブラウザーで、以前にVisitor APIを使用していたページ（現在はWeb SDKを使用）を開きます。
2. 開発者ツールで、`kndctr_` ID Cookieが設定され、ECIDが`AMCV_` CookieのIDと一致することを確認します。
3. デプロイメント後、移行前と移行後の同じ期間のユニーク訪問者数を比較します。 ユニーク訪問者が大幅に増加した場合、IDが移行されないことを示している可能性があります。

>[!TIP]
>
>`getIdentity`を使用して、比較用にECIDをプログラムで取得します。
>
>```js
>alloy("getIdentity", { namespaces: ["ECID"] }).then(function(result) {
>   console.log("ECID:", result.identity.ECID);
>});
>```

## 設定 `idMigrationEnabled` {#configure}

`idMigrationEnabled` コマンドの実行時に`configure` ブール値を設定します。 Web SDKの設定時にこのプロパティを省略すると、デフォルトは`true`になります。 Visitor APIによって設定されたAMCV Cookieの読み取り機能を無効にする場合は、このプロパティを設定します。 ほとんどの組織では、このプロパティを設定する必要はありません。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
  idMigrationEnabled: false
});
```

## Web SDK タグ拡張機能を使用した訪問者IDの移行の有効化

これらの設定は、[ID設定](/help/tags/extensions/client/web-sdk/configure/identity.md)を使用して、Web SDK タグ拡張機能で設定できます。
