---
title: データ収集でのidentityMapの使用
description: Web SDKの実装で、名前空間をまたいで既知の訪問者を識別するためにIdentityMap ペイロードを構築して送信する方法について説明します。
source-git-commit: 696e5098ebf556bfc0fa4fc22ff637cb0835eee0
workflow-type: tm+mt
source-wordcount: '1671'
ht-degree: 0%

---

# データ収集でのidentityMapの使用

`identityMap` ペイロードオブジェクトは、訪問者がデバイスレベル [ECID](./overview.md)を超えているEdge Networkを識別する方法です。 訪問者がログインしたり、購入を完了したり、その他の方法で既知になったりすると、個人レベルのID （CRM ID、ハッシュメール、ロイヤルティ IDなど）をECIDと一緒に送信できます。 これらの個人レベルのIDは、下流のサービスに貴重な情報を提供し、次のことを可能にします。

* **複数のデバイスとチャネルをまたいでアクティビティを個人に結び付けます。** [ID サービス ](/help/identity-service/home.md)は、送信したIDを[ID グラフ ](/help/identity-service/features/identity-graph-viewer.md)にリンクし、匿名のデバイス レベルの動作を既知の人物に接続します。
* **統合顧客プロファイルの構築。** [ リアルタイム顧客プロファイル ](/help/profile/home.md)は、イベントと属性を単一のプロファイルに固定するために設定したプライマリ IDを使用し、個人レベルのセグメンテーションとオーディエンスの構築を可能にします。
* **下流の宛先でオーディエンスをアクティブ化します。**&#x200B;多くの[宛先](/help/destinations/home.md)では、オーディエンスをユーザーベースに一致させるために、解決された個人レベルのID （ハッシュ化された電子メール、電話番号など）が必要です。
* **クロスチャネルジャーニーのオーケストレーション。** [Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/ajo-home.html?lang=ja)は、解決済みのIDを使用して、訪問者の認証済みの行動に基づいて、電子メール、プッシュ通知、アプリ内チャネルをまたいでジャーニーをトリガーし、パーソナライズします。

このページでは、`identityMap` ペイロードを構築し、各IDに適切な設定を選択し、一般的な実装シナリオを処理する方法について説明します。

## ペイロード構造 {#structure}

`identityMap`はJSON オブジェクトです。各トップレベル キーは名前空間で、値はID記述子の配列です。 1つのペイロードに1つ以上の名前空間を含めることができ、各記述子には`id`、`authenticatedState`、`primary`の3つのフィールドが含まれます。

```json
{
  "identityMap": {
    "CRMID": [
      {
        "id": "abc-123-xyz",
        "authenticatedState": "authenticated",
        "primary": true
      }
    ]
  }
}
```

| フィールド | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| `id` | 文字列 | ○ | 識別子の値（例：`user@example.com`または`ABC123`）。 |
| `authenticatedState` | 文字列 | ○ | このIDが訪問者のものであることを自信を持って知っています。 有効な値には、`ambiguous`、`authenticated`および`loggedOut`が含まれます。 |
| `primary` | ブール | ○ | このIDをイベントのプライマリ IDにするかどうか。 すべての名前空間に対して1つのIDを正確にマークする必要があります`primary: true`。 |

以下のセクションでは、ペイロードの各部分について詳しく説明します。

### 名前空間キー {#namespace-keys}

`identityMap`の各トップレベル キーは[ID名前空間](/help/identity-service/features/namespaces.md)です。識別子の種類を分類する文字列（`CRMID`、`Email`、`Phone`、または`LoyaltyId`など）です。 名前空間は、ペイロードで参照する前にID サービスに存在する必要があります。 訪問者に複数の識別子がある場合、同じイベントに複数の名前空間キーを含めることができます。

ECIDを名前空間キーとして含める必要はありません。 Edge Networkは、ECIDをID ペイロードに自動的に追加します。 ECIDを明示的に含める場合は、個人レベルのIDも存在する場合は、`primary`としてマークしないでください。

### `id` {#id}

`id` フィールドは、識別子文字列自体です。このIDがどの特定の個人またはデバイスを表しているかをExperience Platformに伝える値です。 各[ID名前空間](/help/identity-service/features/namespaces.md)には特定の値の形式が必要です。誤った形式で値を送信すると、正しい形式のバージョンと結合できない個別のIDが作成され、プロファイルが断片化されます。

`identityMap`に値を含める前に、ターゲット名前空間で想定される形式に従って値を準備します。

| 一般的な名前空間タイプ | 価値創出のための準備方法 | 例 |
| --- | --- | --- |
| **CRM / 内部ID** | 記録システムが割り当てた正確なIDを使用します。 すべてのイベント（大文字と小文字、先頭のゼロ、接頭辞）で形式の一貫性を維持します。 | `ABC-12345`, `00098765` |
| **電子メール （未加工）** | 電子メールアドレス全体を小文字にし、先頭と末尾の空白をトリミングします。 | `user@example.com` |
| **電子メール （ハッシュ化）** | 最初にメールアドレスを小文字にしてトリミングし、次にSHA-256でハッシュします。 結果の64文字の16進文字列を送信します。 名前空間の定義が必要でない限り、saltを追加しないでください。 | `a1b2c3d4e5f6a7b8c9...` |
| **電話（E.164）** | [E.164](https://en.wikipedia.org/wiki/E.164)の数値を書式設定します。先頭の`+`、国コード、およびスペースや句読点のない購読者番号を指定します。 | `+15551234567` |
| **FPID** | [UUIDv4](https://datatracker.ietf.org/doc/html/rfc4122)文字列を生成します。 生成要件については、[ ファーストパーティデバイス ID](./fpid.md)を参照してください。 | `123e4567-e89b-42d3-9456-426614174000` |

標準の名前空間とその定義の完全なリストについては、[ID名前空間の概要](/help/identity-service/features/namespaces.md#standard)を参照してください。

>[!TIP]
>
>`id`値では大文字と小文字が区別されます。 `User@Example.com`と`user@example.com`は2つの別々のIDとして扱われます。 値を送信する前にケーシングを正規化し（通常は電子メールを小文字にし、空白をトリミングします）、グラフに重複するIDが作成されないようにします。

#### 実行時に`id`を収集しています {#collect-id}

必要な識別子がページ上で直接利用可能になることはほとんどありません。 一般的な収集戦略には、次のものがあります。

* **データレイヤー**：訪問者がログインした後、サイトのデータレイヤーから識別子を読み取ります。 この場所は、データレイヤーがアプリケーションのバックエンドによって入力され、認証済みセッションの状態を反映するため、最も信頼性の高いアプローチです。
* **認証トークンまたはセッション Cookie**：認証システムが設定したJWTまたはセッション Cookieから識別子をデコードまたは検索します。 値を使用する前に、トークンがまだアクティブであることを検証します。
* **サーバーサイドのエンリッチメント**: [Data Prep for Data Collection](/help/datastreams/data-prep.md)または[ イベント転送ルール ](/help/tags/ui/event-forwarding/overview.md)を使用して、EdgeでIDをマッピングまたは変換してから、ダウンストリームサービスに到達します。 この場所は、クライアントがサーバーサイドの内部IDにマッピングする不透明なセッショントークンのみを持っている場合に便利です。

>[!TIP]
>
>指定された`id`値が空の文字列、`null`または`undefined`に解決される場合、`identityMap`に名前空間を含めないでください。 空の`id`を送信すると、壊れたID レコードが作成されます。 ペイロードを構築する前に、チェックを行って実装を監視します。

### `authenticatedState` {#authenticated-state}

`authenticatedState` フィールドは、特定のIDにどの程度の信頼度を割り当てるかをダウンストリームサービスに伝えます。 このフィールドには次の値が有効です。

| 値 | 使用するタイミング |
| --- | --- |
| **`authenticated`** | 訪問者は、現在のセッション中に積極的にIDを証明しました（資格情報を使用したログイン、MFAの完了、または同様の検証など）。 |
| **`loggedOut`** | 訪問者は以前に認証されましたが、その後ログアウトしています。 IDは引き続きデバイスに関連付けられますが、セッションはアクティブではありません。 |
| **`ambiguous`** | IDが現在の訪問者に属していることを確認できません。 この値は、FPIDなどのデバイスレベルの識別子や、まだ認証が行われていない識別子に使用します。 |

>[!TIP]
>
>`authenticatedState`値は、Adobe Experience Platform Identity ServiceがID グラフを構築および結合する方法に影響します。 確認されていないIDに`authenticated`を送信すると、取り消しにくい不正なクロスデバイスリンクが作成される可能性があります。

### `primary` {#primary-identity}

`identityMap` ペイロードごとに、`primary: true`としてマークされたIDを1つだけ持つ必要があります。 プライマリ IDは、Experience Platformでイベントのアンカーとして使用されるIDを決定します。

プライマリ IDを設定する場合は、次のガイドラインに従ってください。

* **個人レベルのIDが使用可能な場合** （訪問者がログインしている場合）、個人レベルの名前空間をプライマリとして、ECIDを非プライマリとしてマークします。 これにより、Experience Platformは、デバイスではなく個人にイベントを固定するように指示されます。
* **デバイスレベルのIDのみが使用可能な場合** （訪問者が匿名の場合）、ECIDがプライマリ IDとして自動的に使用されます。 `identityMap`にECIDを含める必要はありません。Edge Networkが自動的に追加します。

```json
{
  "identityMap": {
    "CRMID": [
      {
        "id": "abc-123-xyz",
        "authenticatedState": "authenticated",
        "primary": true
      }
    ],
    "Email": [
      {
        "id": "user@example.com",
        "authenticatedState": "authenticated",
        "primary": false
      }
    ]
  }
}
```

## 実装でのIDMapの送信 {#send-identity}

>[!BEGINTABS]

>[!TAB JavaScript library]

`identityMap`呼び出しの`xdm` オブジェクトに[`sendEvent`](/help/collection/js/commands/sendevent/overview.md)を渡します。

```js
alloy("sendEvent", {
  xdm: {
    identityMap: {
      CRMID: [
        {
          id: "abc-123-xyz",
          authenticatedState: "authenticated",
          primary: true
        }
      ]
    },
    eventType: "web.webpagedetails.pageViews"
  }
});
```

>[!TAB Web SDK タグ拡張機能]

タグ UIでID ペイロードを構築するには、[ID マップ ](/help/tags/extensions/client/web-sdk/data-element-types.md#identity-map) データ要素タイプを使用します。

1. **[!UICONTROL Adobe Experience Platform Web SDK]**&#x200B;拡張機能と&#x200B;**[!UICONTROL Identity map]** データ要素タイプを使用してデータ要素を作成します。
2. 名前空間、識別子に解決するデータ要素または値、および認証状態を指定して、IDを追加します。
3. 1つのIDをプライマリとしてマークします。
4. このデータ要素は、**[!UICONTROL Send event]**&#x200B;の下の&#x200B;**[!UICONTROL Identity map]** アクションで参照してください。

>[!ENDTABS]

## 一般的なシナリオ {#common-scenarios}

+++**ログイン**

訪問者がログインしたら、個人レベルのIDを`authenticatedState: "authenticated"`と`primary: true`で送信します。 このIDは、認証後の最初のイベントおよびセッション内のその後のすべてのイベントに含めます。

```json
{
  "identityMap": {
    "CRMID": [
      {
        "id": "abc-123-xyz",
        "authenticatedState": "authenticated",
        "primary": true
      }
    ]
  }
}
```

+++

+++**ログアウト**

訪問者がログアウトしたら、同じIDを維持しながら`authenticatedState`を`loggedOut`に更新します。 これにより、デバイスとユーザーとの関連付けが保持され、セッションがアクティブでなくなったことを示します。

```json
{
  "identityMap": {
    "CRMID": [
      {
        "id": "abc-123-xyz",
        "authenticatedState": "loggedOut",
        "primary": false
      }
    ]
  }
}
```

ログアウト後、ECIDは有効なプライマリ IDに戻ります（Edge Networkでは自動的に適用されます）。 訪問者が別のアカウントでログインしない限り、別の個人レベル IDをプライマリとしてマークしないでください。

>[!IMPORTANT]
>
>ログアウト後にIDの送信を完全に停止しないでください。 `authenticated`から`loggedOut`に切り替えると、セッションが終了したことをダウンストリームサービスに伝えます。 識別子を完全に省略すると、ID グラフにギャップが残り、プロファイルが断片化する可能性があります。

+++

+++**複数の名前空間**

同じイベントで複数のID名前空間を送信できます。 このシナリオは、訪問者がログインしており、複数の識別子（CRM ID、ハッシュ化された電子メール、ロイヤルティ IDなど）が使用可能な場合に一般的です。 すべての既知のIDを含め、1つのみをプライマリとしてマークし、他のIDを`primary: false`に設定します。

```json
{
  "identityMap": {
    "CRMID": [
      {
        "id": "abc-123-xyz",
        "authenticatedState": "authenticated",
        "primary": true
      }
    ],
    "Email_LC_SHA256": [
      {
        "id": "a1b2c3d4e5f6...",
        "authenticatedState": "authenticated",
        "primary": false
      }
    ],
    "LoyaltyId": [
      {
        "id": "LOY-98765",
        "authenticatedState": "authenticated",
        "primary": false
      }
    ]
  }
}
```

>[!TIP]
>
>同じイベントで同じ`authenticatedState`を持つ複数の名前空間を送信すると、ID サービスは、これらのIDをリンクするための最も強力なシグナルを提供します。 利用可能なすべての識別子を、別々のイベントに分散させるのではなく、認証の時点で含めます。

+++

+++**匿名の訪問者**

匿名の訪問者の場合、通常は`identityMap`を送信する必要はありません。 Edge NetworkはECIDを自動的に割り当て、プライマリ IDとして使用します。 [ ファーストパーティデバイス ID](./fpid.md)を使用する場合、匿名の訪問者に含める必要があるIDはFPIDのみです。

+++

## identityMapがID グラフに与える影響 {#identity-graph}

Experience Platformに到達する`identityMap`個のペイロードは[ID サービス ](/help/identity-service/home.md)によって処理され、送信したIDが[ID グラフ ](/help/identity-service/features/identity-graph-viewer.md)にリンクされます。 どの名前空間を含めるか、どのように`authenticatedState`を設定するか、そしてどのIDを`primary`としてマークするかは、Identity Serviceがそれらのグラフを構築および結合する方法を直接形作ります。

注意すべき主な行動：

* 同じイベントで送信された&#x200B;**IDはリンクされています。**&#x200B;同じ`sendEvent`呼び出しにCRMIDとメール名前空間を含める場合、ID サービスはこれらの2つのID間のリンクを作成します。 識別子を別々のイベントに分散させると、リンクが弱くなり、グラフが断片化する可能性があります。
* **`primary` IDは、リアルタイム顧客プロファイルでイベントを固定します。** プロファイルは、プライマリ IDを使用して、イベントがどのプロファイルに属しているかを判断します。 誤ったIDをプライマリとしてマークする（例えば、個人レベル IDが使用可能な場合にECIDをプライマリとして設定する）と、個人レベルのプロファイルではなく、デバイスレベルのプロファイルに対してイベントが保存される可能性があります。
* **`authenticatedState`はグラフの信頼性に影響します。**&#x200B;実際に確認されていないIDに対して`authenticated`を送信すると、取り消しにくい不正なクロスデバイスリンクが作成される可能性があります。 訪問者が現在のセッション中に積極的にIDを証明した場合にのみ`authenticated`を使用します。

実装で[ID グラフ リンク ルール ](/help/identity-service/identity-graph-linking-rules/overview.md) （名前空間の優先順位やID最適化アルゴリズムなど）を使用している場合は、[実装ガイド ](/help/identity-service/identity-graph-linking-rules/implementation-guide.md)を参照して、これらのルールが`identityMap`を通じて送信するIDとどのように相互作用するかを理解してください。

>[!NOTE]
>
>`identityMap`は、Web SDKが訪問者の同意状態によってゲートされるEdge Networkにリクエストを行った場合にのみ送信されます。 実装で`defaultConsent: "pending"`を使用している場合、同意が付与されるまでIDは送信されません。 詳しくは、[同意とID](./consent.md)を参照してください。
