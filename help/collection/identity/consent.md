---
title: データ収集における同意とID
description: ECIDの生成、Cookieの永続化、訪問者の継続性など、Web SDKの実装におけるID行動に同意の選択が与える影響を理解できます。
source-git-commit: 696e5098ebf556bfc0fa4fc22ff637cb0835eee0
workflow-type: tm+mt
source-wordcount: '1149'
ht-degree: 2%

---

# データ収集における同意とID

Web SDKの導入では、同意とIDは密接に関連しています。 同意を収集する方法とタイミングは、Web SDKがECIDを生成し、ID Cookieを設定し、データをEdge Networkに送信するタイミングとタイミングに直接影響します。 同意を慎重に処理しないと、訪問者の予期しないインフレーション、IDの連続性のギャップ、またはその両方が発生することがよくあります。

このページでは、同意の選択がIDの行動とどのように関わるのかを説明し、一般的な落とし穴を回避するために実装を設定するためのガイダンスを提供します。 Web SDKがECID、FPID、およびその他のID シグナルを処理する方法の背景については、「[&#x200B; データ収集のID](./overview.md)」を参照してください。

## 同意がIDに与える影響 {#how-consent-affects-identity}

Web SDKでは、[`defaultConsent`](/help/collection/js/commands/configure/defaultconsent.md)構成変数と[`setConsent`](/help/collection/js/commands/setconsent.md) コマンドの両方を使用して、データをEdge Networkに送信するかどうかを制御します。 同意ステータスは、ECIDが生成されるタイミングとID Cookieが設定されるタイミングを直接決定します。

次の表は、データ収集、Cookie設定、およびID動作に対する`defaultConsent`と`setConsent`の複合効果を示しています。

| `defaultConsent` | `setConsent` | データ収集の発生 | ブラウザーのCookie セット | ID行動 |
| --- | --- | --- | --- | --- |
| `in` | 設定なし | ○ | ○ | ECIDは、最初のリクエストで即座に生成されます。 ID Cookieはページ読み込み時に設定されます。 |
| `in` | `in` | ○ | ○ | 訪問者の既存のECIDは保持されます。 IDの動作は変更されません。 |
| `in` | `out` | × | ○ | データ収集の停止： 既存のECIDおよび`kndctr_` ID Cookieは、有効期限が切れるまでブラウザーに残ります。 |
| `pending` | 設定なし | × | × | ECIDは生成されません。 Cookieが設定されていません。 イベントは、`setConsent`が呼び出されるまでローカルでキューに入れられます。 |
| `pending` | `in` | ○ | ○ | キューに入ったイベントが送信されます。 最初のリクエストでECIDが生成され、ID Cookieが設定されます。 |
| `pending` | `out` | × | ○ | キューに入れたイベントは破棄されます。 ECIDは生成されません。 同意Cookieは、訪問者の好みを記録するように設定されます。 |
| `out` | 設定なし | × | × | ECIDは生成されません。 Cookieが設定されていません。 イベントは送信されません。 |
| `out` | `in` | ○ | ○ | 最初のリクエストでECIDが生成され、ID Cookieが設定されます。 |
| `out` | `out` | × | ○ | ECIDは生成されません。 同意Cookieは、訪問者の好みを記録するように設定されます。 |

>[!NOTE]
>
>訪問者がオプトアウトした場合でも、IDおよび同意Cookieが設定されます。 こうしたCookieは、訪問者のデータ収集の好みを尊重するために必要です。 Web SDKが設定するCookieの完全なリストについては、[Web SDK Cookie](https://experienceleague.adobe.com/en/docs/core-services/interface/data-collection/cookies/web-sdk)を参照してください。

以前に同意を取り消した後に訪問者が同意を再付与した場合（`setConsent`の後`"general": "in"`で`"general": "out"`を呼び出すことにより）、Web SDKはイベントの送信を再開し、有効期限が切れていない場合はCookieから既存のECIDを使用します。 訪問者のIDは保持されます。

訪問者が同意を付与または拒否した後、Web SDKは`kndctr_`個の同意Cookieに自身の環境設定を保持します。 後続のページ読み込みでは、SDKはこのCookieを読み取り、保存された環境設定を自動的に適用します。訪問者の環境設定が変更されない限り、`setConsent`を再度呼び出す必要はありません。 `defaultConsent`設定値はページ読み込み間に保持されませんが、訪問者の解決済み同意（`setConsent`で設定）は保持されます。

>[!NOTE]
>
>同意が`pending`である間にキューに入れられたイベントはメモリに保持され、ページの再読み込みに耐えられません。 同意が解決される前に訪問者が新しいページに移動すると、前のページからのキューに入れられたイベントは失われます。

## 実装パターン {#implementation-patterns}

### オプトインモデル（収集前に同意が必要） {#opt-in}

このパターンは、規制（GDPRなど）がデータを収集する前に明示的な同意を必要とする場合に使用します。

```js
alloy("configure", {
  orgId: "YOUR_ORG_ID@AdobeOrg",
  edgeDomain: "data.example.com",
  defaultConsent: "pending"
});

// When the visitor grants consent:
alloy("setConsent", {
  consent: [{
    standard: "Adobe",
    version: "1.0",
    value: { general: "in" }
  }]
});
```

このパターンで：
* 同意が付与されるまで、ECIDは生成されません。
* 同意前にトリガーされたイベント（最初のページビューなど）は、同意が付与された後にキューに入れられ、送信されます。
* ID Cookieは、最初のEdge Network リクエストが成功した後にのみ設定されます。

### オプトアウトモデル（デフォルトで収集、拒否で停止） {#opt-out}

このパターンは、規制でデータ収集がデフォルトで許可され、オプトアウトのオプションがある場合に使用します。

```js
alloy("configure", {
  orgId: "YOUR_ORG_ID@AdobeOrg",
  edgeDomain: "data.example.com",
  defaultConsent: "in"
});

// If the visitor opts out:
alloy("setConsent", {
  consent: [{
    standard: "Adobe",
    version: "1.0",
    value: { general: "out" }
  }]
});
```

このパターンで：
* ECIDは、最初のページ読み込み時に即座に生成されます。
* 訪問者がオプトアウトするまで、すべてのイベントが送信されます。
* オプトアウト後、Web SDKはイベントの送信を停止しますが、既存のCookieは残ります。

## ファーストパーティデバイス IDを使用した同意 {#consent-with-fpids}

実装で[&#x200B; ファーストパーティデバイス ID （FPID） &#x200B;](./fpid.md)を使用する場合、FPID Cookieは、Web SDKの同意状態とは関係なく、サーバーによって設定されます。 FPID Cookieは、独自のインフラストラクチャで管理する識別子です。 ただし、FPIDは、Web SDKがリクエスト（同意によってゲーテッド）を行った場合にのみEdge Networkに送信されます。

* `defaultConsent: "pending"`では、FPIDはブラウザーに存在しますが、同意が付与されるまでECIDのシード処理には使用されません。
* `defaultConsent: "in"`を使用すると、FPIDは最初のリクエストで使用され、ECIDをすぐにシードします。

同意の実装で、同意の前に識別子を設定する必要がない場合は、同意が伝わるまでFPID Cookieの設定を遅らせてください。 Web SDKの同意ゲートだけでは、FPID Cookieはサーバーによって管理されるため、設定されるのを防ぐことはできません。

## よくある落とし穴 {#common-pitfalls}

+++**同意バナーがID Cookieをクリアします**

**問題**：一部の同意管理プラットフォーム（CMP）は、訪問者が選択を行う前に、同意バナーを表示する際に、`kndctr_`個のID Cookieを含むすべてのCookieをクリアします。 訪問者が同意を付与すると、以前のECIDが削除されたため、Web SDKは新しいECIDを生成します。 訪問者がレポートに新しい人物として表示されます。

**症状**:
* 同意バナーをデプロイすると、ユニーク訪問者数が急増。
* 再訪問者は、同意が失効するたびに新規訪問者としてカウントされ、バナーを再度操作します。

**解決策**: `kndctr_` Cookieを保持するようにCMPを設定します。 これらのCookieはID Cookieであり、トラッキング Cookieではありません。デバイスを識別するもので、行動データは含まれていません。 CMPでCookieを消去する必要がある場合は、除外リストに`kndctr_`個の接頭辞Cookieを追加します。 または、訪問者が同意を明確に拒否するまで、Cookieの消去を先回りして消去するのではなく、遅らせることもできます。

+++

+++**同意が遅れると、IDが重複します**

**問題**: `defaultConsent`が`pending`に設定されている場合、Web SDKは同意を待ってからデータを送信します。 ページのライフサイクルの後半で同意が付与された場合（例えば、ページのリロードをトリガーするバナーインタラクションの後など）、次の順序で問題が発生する可能性があります。

1. ページの読み込み： `defaultConsent: "pending"`。Web SDKはリクエストを送信しません。
2. 訪問者が同意を付与します。 CMPは、ページのリロードをトリガーします。
3. ページが再度読み込まれます。 Web SDKは、現在の同意を得て初期化され、ECIDを生成します。

このフローは正常で、正しく機能します。 この問題は、CMPまたは実装が手順2と3の間で誤ってCookieをクリアする場合、またはリロード時にWeb SDKの設定が異なる場合に発生します。

**解決策**: Web SDKの設定（特に`orgId`と`defaultConsent`）が、ページ読み込みのたびに同じであることを確認します。 CMPが同意後にリロードをトリガーする場合は、ID Cookieがリロード後も有効であることを確認します。

+++

+++**同意バナーで`defaultConsent: "in"`を使用**

**問題**：一部の実装は`defaultConsent: "in"`を設定し、訪問者が辞退した場合は`setConsent`で`"general": "out"`を呼び出します。 このアプローチでは、ECIDを生成し、同意が拒否される前に少なくとも1つのリクエストを送信します。 規制要件によっては、この最初のデータ収集が組織のプライバシーポリシーに一致しない場合があります。

**解決策**：データの収集またはECIDの生成の前に規制環境で同意が必要な場合は、代わりに`defaultConsent: "pending"`を使用してください。 この設定により、同意が明示的に付与されるまで、Web SDKがEdge Networkと通信しないようにします。

+++
