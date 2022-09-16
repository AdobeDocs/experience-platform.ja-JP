---
title: Adobe クライアントデータレイヤー拡張機能
description: Adobe Experience Platform における Adobe クライアントデータレイヤーのタグ拡張について説明します。
exl-id: c4d1b4d3-4b51-4701-be2e-31b08e109bf6
source-git-commit: 27e5c64f31b9a68252d262b531660811a0576177
workflow-type: tm+mt
source-wordcount: '643'
ht-degree: 100%

---

# Adobe クライアントデータレイヤー拡張機能

このドキュメントでは、Adobe クライアントデータレイヤー拡張機能の使用例とベストプラクティスを説明します。

<!-- (Missing document?)
If you would like to have more details on development consideration, [please reach this page](./dev.md). -->

## インストール

拡張機能をインストールするには、データ収集 UI の拡張機能カタログに移動し、「Adobe Client Data Layer」を選択します。

![カタログ内の ACDL 拡張機能ビュー](./images/catalog.png)

<!-- (GitHub link?)
There is also the possibility to fork this project. You can download this github project, realize the change that you deem required for your specific use-case and re-upload it on your Organization as a private extension.
This installation will not be supported on our end.<br>
>[!NOTE]
>
> _Consider renaming the extension name in the extension.json file_ -->

## 拡張機能ビュー

ACDL スクリプトは、デフォルトでは、変数名 `adobeDataLayer` で新しいデータレイヤーを作成します。この名前は、拡張機能ビューで必要に応じて変更できます。設定した名前は、タグが読み込まれる際にインスタンス化されます。

>[!NOTE]
>
>オブジェクト名を変更する場合、元の `adobeDataLayer` オブジェクトは引き続きインスタンス化され、選択した新しい変数名に複製されます。

## イベント

拡張機能を使用すると、データレイヤー上のイベントをリッスンできます。 使用できるイベントは次のとおりです。

### すべてのデータの変更をリッスン

このオプションを選択すると、イベントリスナーはこのデータレイヤーに加えられた変更をリッスンします。

>[!IMPORTANT]
>
>イベントをプッシュしても、データレイヤー自体は変更されません。

次のプッシュイベントの例は、リスナーによってトラッキングされます。

* ` adobeDataLayer.push({"data":"something"})`
* ` adobeDataLayer.push({"event":"myevent","data":"something"})`

次のプッシュイベントの例は、リスナーによってトラッキングされません。

* ` adobeDataLayer.push({"event":"myevent"})`

### すべてのイベントをリッスン

このオプションを選択すると、イベントリスナーは、データレイヤーにプッシュされたすべてのイベントをリッスンします。

次のプッシュイベントの例は、リスナーによってトラッキングされます。

* ` adobeDataLayer.push({"event":"myevent"})`
* ` adobeDataLayer.push({"event":"myevent","data":"something"})`

次のプッシュイベントの例は、リスナーによってトラッキングされません。

* ` adobeDataLayer.push({"data":"something"}) `

### 特定のイベントをリッスン

イベントを指定すると、イベントリスナーで特定の文字列に一致するイベントをトラッキングできます。

例えば、`myEvent` を指定してこの設定を使用すると、リスナーは次のプッシュイベントのみをトラッキングします。

* `adobeDataLayer.push({"event":"myEvent"})`

イベントリスナーの範囲を変更することもできます。オプションの違いの概要を次に示します。

* `all`：デフォルトのオプションです。上記で選択した条件が過去に満たされるか、将来プッシュされるたびにルールをトリガーします。非同期実装を使用する場合に最も安全なオプションです。
* `future`：このオプションは、条件に一致する新しいプッシュイベントがデータレイヤーに送信される場合にのみ、ルールをトリガーします。
* `past`：このオプションは、トリガー条件に一致する古いプッシュイベントに対してのみルールを設定します。条件に一致する新しいプッシュは無視され、ルールはトリガーされません。

## アクション

以下の節では、拡張機能がサポートするアクションの概要を説明します。

### データレイヤーをリセット

拡張機能を使用すると、データレイヤーの長さをリセットでき、シングルページアプリケーション（SPA）で限られたサイズを維持するのに役立ちます。

ただし、前にプッシュメソッド中に設定した情報が完全に削除される可能性は、現在のところありません。

「**計算済み状態のリセットと設定**」アクションは、最後に計算された状態をコピーし、データレイヤーオブジェクトを空にして、最後の状態を再プッシュします。

### データレイヤーにプッシュ

拡張機能を使用すると、JSON コンテンツをデータレイヤー自体にプッシュできます。このアクションにより、JSON 内で直接データ要素を使用できます。 JSON エディター内では、データ要素はパーセント表記（`%dataElementName%` など）を使用して参照する必要があります。

```json
{
    "page": {
        "url": "%url%",
        "previous_url": "%previous_url%",
        "concatenated_values": "static string %dataElement%"
    }
}
```

## データ要素

以下の節では、拡張機能が提供するユニークなデータ要素のタイプについて説明します。

### 計算済み状態

データレイヤーの計算済み状態のデータ要素は、設定に応じて、次の 2 つのいずれかを返すことができます。

* データレイヤーの完全な状態：デフォルトでは、データレイヤーの完全な計算状態を返します。
* 特定のパス：データレイヤーで返すパスを指定できます。パスは、ドット表記（`data.foo` など）を使用して指定します。

### データレイヤーサイズ

このデータ要素は、データレイヤーのサイズを返します。 データレイヤーのサイズは、このオブジェクトにプッシュされた要素の数で表されます。

以下のプッシュイベントのリストの場合、データ要素は整数 `2` を返します。

```js
adobeDataLayer.push({"event":"myEvent"})
adobeDataLayer.push({"data":{"foo":"bar"}})
```
