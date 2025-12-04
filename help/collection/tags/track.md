---
title: track （）
description: 直接呼出しルールをトリガーします。
source-git-commit: a36e5af39f904370c1e97a9ee1badad7a2eac32e
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 2%

---

# `track()`

`_satellite.track()` メソッドを使用すると、[&#x200B; 直接呼び出しルール &#x200B;](/help/tags/extensions/client/core/overview.md#direct-call-event) をトリガーに設定できます。

>[!IMPORTANT]
>
>Adobeでは、`_satellite.track()` を **レガシー実装方式** と見なしています。 Adobeでは、引き続き完全にサポートされますが、新規実装に推奨されるアプローチである [Adobe Client Data Layer](/help/tags/extensions/client/client-data-layer/overview.md) など、より最新の実装方法を使用することを強くお勧めします。
>
>サイトでの `_satellite.track()` の使用を選択する場合は、サイトがタグライブラリに緊密に結び付けられないように **すべての呼び出しを保護** します。 保護されていない場合、今後タグプロパティを削除すると、`_satellite` オブジェクトへのすべての参照でエラーがスローされます。

```ts
_satellite.track(identifier: string, detail?: unknown ): void;
```

タグ UI で設定された識別子を使用して `_satellite.track()` を呼び出すと、そのルールが直ちにトリガーされます。 このメソッドを呼び出すと、ルールイベントとしてのみ機能します。ルールの条件は、ルールのアクションを実行する前に引き続き適用されます。 複数のダイレクトコールルールで同じ ID を使用できるので、1 回の `_satellite.track()` 呼び出しで一度にこれらすべてのルールにトリガーを付けることができます。 複数のルールが同じ識別子を共有している場合でも、トリガーされた各ルールは、アクションを実行する前に独自の条件を確認します。

## 使用可能なフィールド

`_satellite.track()` メソッドでは、次の 2 つの引数をサポートしています。

| 名前 | タイプ | 必須 | 説明 |
|---|---|---|---|
| **`identifier`** | `string` | ○ | 直接呼出しルール識別子。 この識別子は、タグ UI でルールを設定する際に設定します。 |
| **`detail`** | `unknown` | × | 必要な情報を含むオプションのペイロード。 ルールを設定する際に、`event.detail` （カスタムコード）または `%event.detail%` （データ要素表記をサポートするテキストフィールド）を使用してペイロードにアクセスできます。 |

```js
// Trigger rules with the identifier 'example'
if (window._satellite?.track) {
  _satellite.track('example');
}

// Trigger a direct call rule with an optional payload that your tag rule can use
_satellite.track('contact_submit', { name: 'John Doe' });
// When configuring the rule, access the payload field using:
// event.detail.name (custom code block) or
// %event.detail.name% (data element)
```
