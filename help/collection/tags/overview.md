---
title: サテライトオブジェクトのリファレンス
description: クライアントサイドの _satellite オブジェクトと、それを使用してタグで実行できる様々な関数について説明します。
exl-id: f8b31c23-409b-471e-bbbc-b8f24d254761
source-git-commit: 05bf3a8c92aa221af153b4ce9949f0fdfc3c86ab
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 10%

---

# `_satellite` オブジェクト参照

_以下のページでは、`_satellite` オブジェクトの使用方法の概要を説明します。このオブジェクトを使用すると、JavaScriptを使用してタグロジックを管理およびカスタマイズできます。 データ収集 UI で実装を設定する方法について詳しくは [](/help/tags/extensions/client/web-sdk/overview.md)Adobe Experience Platform Web SDK タグ拡張機能 } を参照してください。_

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
// Read and write a temporary data element value (guarded)
if(window._satellite?.getVar && window._satellite?.setVar) {
  const region = _satellite.getVar('user_region');
  _satellite.setVar('promo_code', code);
}

// Manually trigger a rule configured in your tag property (guarded)
if (window._satellite?.track) {
  _satellite.track('cart_add', { sku: '123', qty: 2 });
}

// Local console debugging (guarding not needed)
_satellite.setDebug(true);
_satellite.logger.log('Rule evaluated');
```
