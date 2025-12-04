---
title: _monitors
description: タグ実装をデバッグするためのイベントリスナーを追加します。
source-git-commit: 6f8bdfd09023ea48962a40a9539afe017bc108cc
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 1%

---

# `_monitors`

`_satellite._monitors` オブジェクトを使用すると、イベントリスナーを作成し、ライブラリがトリガーされたルールを検出したときにカスタムコードを実行できます。 主な用途は、ルールが正しくトリガーされるように実装のデバッグを支援することです。

>[!IMPORTANT]
>
>このオブジェクトはデバッグ目的でのみ使用されます。 実稼動ロジックをこのオブジェクトに関連付けないでください。 このオブジェクト内のプロパティや名前の使用は、Adobeでいつでも変更できます。

```ts
_satellite._monitors?: {
  ruleTriggered(event: { rule: Rule }): void;
  ruleCompleted(event: { rule: Rule }): void;
  ruleConditionFailed(event: { rule: Rule; condition: Condition }): void;
}[];
```

次のイベントタイプをリッスンできます。

## `ruleTriggered`

このコールバック関数は、イベントが、ルールの条件とアクションが処理される前にルールをトリガーしたときに呼び出されます。 この関数がトリガーの場合、`ruleCompleted` または `ruleConditionFailed` のトリガーが直後に発生します（相互に排他的）。

## `ruleCompleted`

`ruleCompleted` コールバック関数は、ルールの条件の条件が成功し、ルールのすべてのアクションが実行されたときに、`ruleTriggered` 後に呼び出されます。

## `ruleConditionFailed`

`ruleConditionFailed` コールバック関数は、ルールの条件の少なくとも 1 つが失敗した場合、`ruleTriggered` 後に呼び出されます。

## `Rule` オブジェクト

各コールバック関数は、ルール自体に関する情報を提供する `Rule` オブジェクトを公開します。

```ts
type Rule = {
  id: string;
  name: string;
  events: Event[]; // Rule-specific events
  conditions: Condition[]; // Rule-specific conditions
  actions: Action[]; // Rule-specific actions
}
```

| 名前 | タイプ | 説明 |
|---|---|---|
| **`id`** | `string` | ルールの一意の ID。 |
| **`name`** | `string` | ルールのわかりやすい名前。 |
| **`events`** | `Event[]` | ルールのトリガーとして設定したイベントの配列。 |
| **`conditions`** | `Condition[]` | ルールのトリガーとして設定した条件の配列。 |
| **`actions`** | `Action[]` | ルールがトリガーされたときに実行するように設定したアクションの配列。 |

## 例

タグライブラリローダーを呼び出す前に、次のコードのスニペットを `<head>` タグでHTMLに追加します。

```html
<script>
  window._satellite ??= {};
  window._satellite._monitors ??= [];
  window._satellite._monitors.push({
    ruleTriggered(event) { console.log('Rule triggered', event.rule);},
    ruleCompleted(event) { console.log('Rule completed', event.rule);},
    ruleConditionFailed(event) { console.log('Rule condition failed', event.rule, event.condition);}
  });
</script>
<script src="https://assets.adobedtm.com/.../launch-EN...js" async></script>
```

タグライブラリはページのこの時点ではまだ読み込まれていないので、最初の `_satellite` オブジェクトが作成され、`_satellite._monitors` の配列が初期化されます。 次に、スクリプトはその配列にモニターオブジェクトを追加します。

モニターを使用する際は、次のヒントを考慮してください。

* これらのデバッグメッセージでは、`console.log` の代わりに [`_satellite.logger`](logger.md) が使用されることに注意してください。これは、タグライブラリが読み込まれる前にフックが作成されるからです。
* このコード例には 3 つのコールバック関数がすべて含まれていますが、必須フィールドではありません。 サイトでルールをデバッグする際には、必要なフックのみを含めることができます。
* 同じイベントに対しても、複数のモニターを使用できます。 1 つのイベントに複数のモニターを使用する場合、呼び出し順序は保証されません。
* タグライブラリを読み込む `_monitors` 前に _作成_ ていることを確認します。 ライブラリの読み込み後にこれらのフックを作成すると、その時点からトリガーするルールのみがキャッチされます。
