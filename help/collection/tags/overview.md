---
title: サテライトオブジェクトのリファレンス
description: クライアントサイドの _satellite オブジェクトと、それを使用してタグで実行できる様々な関数について説明します。
exl-id: f8b31c23-409b-471e-bbbc-b8f24d254761
source-git-commit: a36e5af39f904370c1e97a9ee1badad7a2eac32e
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 13%

---

# `_satellite` オブジェクト参照

`_satellite` オブジェクトは、サイトに公開されたタグライブラリとの対話に役立つ、サポートされるいくつかのエントリポイントを公開します。 ローダータグが正しく実装されている場合、すべてのタグデプロイメントで `_satellite` が表示されます。 このオブジェクトの主なユースケースは次の通りです。

* カスタムコードブロック内のタグライブラリ内の使用状況。タグライブラリ自体に対するフルアクセスが可能になります。
* 任意の環境（開発、ステージングまたは実稼動）内でデプロイした実装のデバッグ
* Web サイトに直接実装でき、イベントやタグルールをトリガーするタイミングを完全に制御できます。 新規実装の場合、Adobeでは、[Adobe Client Data Layer](/help/tags/extensions/client/client-data-layer/overview.md) など、より柔軟な戦略を使用することをお勧めします。

>[!IMPORTANT]
>
>サイトコード（`_satellite` など）から `_satellite.track()` を呼び出す場合は、サイトがタグライブラリに緊密に結び付かないように **すべての呼び出しを保護** します。
>
>タグプロパティのカスタムコード内またはブラウザーコンソールでローカルに `_satellite` を使用する場合、保護は必要ありません。

## 一般的な使用例

```js
// Read and write a temporary data element value
const region = _satellite.getVar('user_region');
_satellite.setVar('promo_code', code);

// Local debugging
_satellite.setDebug(true);
_satellite.logger.log('Rule evaluated');

// Manually trigger a rule configured in your tag property
if (window._satellite?.track) {
  _satellite.track('cart_add', { sku: '123', qty: 2 });
}
```
