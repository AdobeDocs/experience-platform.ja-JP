---
title: データ収集でのIDのトラブルシューティング
description: 訪問者のインフレーション、ECIDの不整合、Cookieの競合、FPIDの問題など、Web SDKの実装における一般的なIDの問題を診断します。
source-git-commit: 696e5098ebf556bfc0fa4fc22ff637cb0835eee0
workflow-type: tm+mt
source-wordcount: '1034'
ht-degree: 0%

---

# データ収集でのIDのトラブルシューティング

IDの問題は、多くの場合、実装自体のエラーではなく、ダウンストリームのレポート（訪問者数の増加、プロファイルの断片化、パーソナライゼーションの断片化）で発生します。 このページでは、Web SDKの実装で最も一般的なIDの問題を診断および解決する方法を説明します。 データ収集でのIDの仕組みについて詳しくは、[IDの概要](./overview.md)を参照してください。

## ID値の検査 {#inspect-identity}

特定の問題をトラブルシューティングする前に、Web SDKで使用されている現在のID値を取得します。 [`getIdentity`](/help/collection/js/commands/getidentity.md) コマンドを使用して、ECIDおよびその他のID シグナルを表示します。

```js
alloy("getIdentity", { namespaces: ["ECID", "CORE"] }).then(function(result) {
  console.log("ECID:", result.identity.ECID);
  console.log("CORE ID:", result.identity.CORE);
  console.log("Edge region:", result.edge.regionID);
});
```

ブラウザーの開発者ツールでID値を検査することもできます。

1. **アプリケーション** タブ （Chrome/Edge）または&#x200B;**ストレージ** タブ （Firefox/Safari）を開きます。
2. ドメインの`kndctr_`が先頭に付いたCookieを探します。 `kndctr_<ORG_ID>_AdobeOrg_identity` CookieにECIDが含まれています。
3. 「**Network**」タブを開き、Edge Networkへの`interact`または`collect` リクエストを見つけます。 `identityMap`のリクエストペイロードと、ID ハンドルの応答ペイロードを調べます。

## 一般的な問題 {#common-issues}

+++**訪問者数のインフレーション**

**症状**:Analytics レポートに表示されるユニーク訪問者の数が予想を上回っているか、セッション間で同じユーザーが複数の訪問者として表示されています。

**考えられる原因**:

| 原因 | 特定方法 | 解決策 |
| --- | --- | --- |
| Cookieの短期間保持 | ブラウザーで`kndctr_` Cookieの有効期限を確認します。 7日以内に有効期限が切れる場合、ブラウザーポリシーによってCookieの期間が制限される可能性があります。 | DNS A/AAAA レコードを使用してサーバーから設定された[ ファーストパーティデバイス ID （FPID） ](./fpid.md)を実装し、Cookieの永続性を長くします。 |
| 最初のリクエストでFPIDがありません | ページ読み込み時に最初のEdge Network リクエストを調べます。 FPID Cookieが存在しない場合、Edge Networkは新しいECIDを生成します。 最初のリクエストの後にFPIDが設定されている場合、その最初のリクエストで生成されたECIDは孤立します。 | Web SDKが最初のリクエストを送信する前に、FPID Cookieを設定します。 「[Cookieを設定するタイミング ](./fpid.md#when-to-set-cookie)」を参照してください。 |
| ドメイン間で`orgId`の不一致 | ドメイン間で`orgId`設定値を比較します。 値が一致しない場合は、ID スコープが別々になります。 | 組織内のすべてのドメインで同じ[`orgId`](/help/collection/js/commands/configure/orgid.md)を使用します。 |
| Cookieを削除する同意バナー | 同意が付与される前に同意の実装がすべてのCookieをクリアし、Web SDKが初期化されると、新しいECIDが生成されます。 | 同意バナーを設定して`kndctr_` Cookieを保持するか、同意が確立されるまでWeb SDKの初期化を遅らせます。 [同意とID](./consent.md)も参照してください。 |
| JavaScriptで設定されたFPID Cookie | `document.cookie`を使用して設定されたCookieは、ブラウザの制限（ITP、ETP）の対象となり、有効期間が24時間に制限されることがあります。 | JavaScriptからではなく、DNS A/AAAA レコードを使用して、サーバーからFPID Cookieを設定します。 |

+++

+++**ECIDがページ間で予期せず変更される**

**症状**:ECIDは、同じドメインの異なるページで異なっているか、ページ読み込みごとに変更されます。

**診断手順**:

1. `kndctr_` ID Cookieが両方のページに存在することを確認します。 1つのページに欠けている場合は、Web SDKがそのページで設定されていることを確認します。
2. Cookie ドメインが十分に広く設定されていることを確認します。 `shop.example.com`に設定されたCookieは、`www.example.com`には使用できません。 ファーストパーティの収集とCookie設定インフラストラクチャで同じドメインスコープを使用することを確認します。
3. ナビゲーション時にCookieをクリアするJavaScriptを確認します（例：攻撃的なCookie同意スクリプトやプライバシーツール）。
4. シングルページアプリケーションを使用する場合は、Web SDKがアプリケーションの初期化時に1回設定され、すべてのルート変更で再初期化されないことを確認します。 再初期化すると、新しいECIDを生成できます。

+++

+++**FPIDはECIDをシードしていません**

**現象**: FPID Cookieを設定しましたが、`getIdentity`は訪問全体で一貫性のないECIDを返すか、FPIDがEdge Network リクエストペイロードに表示されません。

**診断手順**:

1. **FPID Cookie形式を確認してください**: FPIDは有効な[UUIDv4](https://datatracker.ietf.org/doc/html/rfc4122)である必要があります。 ブラウザーの開発者ツールを開き、FPID Cookieを見つけ、値がパターン `xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx`と一致することを確認します。
2. **データストリームのCookie名を確認**: [ データストリームのCookie メソッド ](./fpid.md#setting-cookie-datastreams)を使用している場合、データストリームで設定されたCookie名は、サーバーが設定したCookieの名前と完全に一致する必要があります。
3. **リクエストでCookieが送信されていることを確認します**:「ネットワーク」タブで、Edge Network リクエストの`Cookie` ヘッダーを調べます。 FPID Cookieを含める必要があります。
4. **IDの優先度を確認**：既存のECIDが既に`kndctr_` Cookieに保存されている場合は、FPIDよりも優先されます。 FPIDは、既存のECIDが存在しない場合にのみ新しいECIDをシードします。 優先度の完全な順序については、[FPIDの仕組み](./fpid.md#how-fpids-work)を参照してください。
5. **CNAMEの検証**: データストリーム cookie メソッドを使用する場合は、ファーストパーティコレクション CNAMEが正しく設定されていること、およびリクエストがルーティングされていることを確認します。

+++

+++**クロスドメイン IDが機能しません**

**現象**：いずれかのドメインから別のドメインにクリックした訪問者は、宛先ドメインの新しい訪問者として扱われます。

**診断手順**:

1. **URLを確認**：訪問者がリンクをクリックしたときに、宛先URLを調べます。 `adobe_mc` クエリ文字列パラメーターを含める必要があります。 パラメーターが見つからない場合、ソースドメインはパラメーターを追加しません。 [ クロスドメイン共有の実装](./cross-domain-sharing.md#implement-cross-domain-sharing)を参照してください。
2. **タイミングを確認**: `adobe_mc` パラメーターは5分後に有効期限が切れます。 宛先ページの読み込みに時間がかかりすぎる場合（リダイレクトやネットワークの速度が遅いなど）、パラメーターはWeb SDKが読み込む前に期限切れになる可能性があります。
3. **一致する`orgId`を確認**：両方のドメインで同じ[`orgId`](/help/collection/js/commands/configure/orgid.md)を使用する必要があります。 組織IDが一致しない場合、宛先ドメインはハンドオフ IDを拒否します。
4. **Web SDKが宛先**&#x200B;にあることを確認します。宛先ページには、Web SDKがインストールされ、設定されている必要があります。 これを指定しない場合、`adobe_mc` パラメーターは無視されます。
5. **URL ストリッピングの確認**：一部のリダイレクトサービス、CDN、またはサーバーサイド ロジック ストリップの不明なクエリ文字列パラメーター。 ソース ページと宛先ページ間の中間リダイレクトが`adobe_mc`で維持されていることを確認します。

+++

+++**モバイルからWebへのIDの引き継ぎが失敗しています**

**症状**: モバイルアプリで開始し、WebViewまたはモバイルブラウザーを開いた訪問者は、Web側では新しい訪問者として扱われます。

**診断手順**:

1. **URLを確認**: WebViewに渡されるURLをログに記録します。 `adobe_mc`[`getUrlVariables`によって生成された](https://developer.adobe.com/client-sdks/edge/identity-for-edge-network/api-reference/#geturlvariables) パラメーターを含める必要があります。
2. **SDKのバージョンを確認**: Edge Network拡張機能のモバイル IDはバージョン 1.1.0以降、Web SDKはバージョン 2.11.0以降である必要があります。
3. **タイミングを確認**: ドメイン間の共有と同様に、`adobe_mc` パラメーターは5分後に有効期限が切れます。 URLの構築後、WebViewが迅速に読み込まれることを確認します。
4. **一致する`orgId`を確認**: Experience Cloud組織IDは、モバイル SDKとWeb SDKの両方の設定で同じである必要があります。

+++
