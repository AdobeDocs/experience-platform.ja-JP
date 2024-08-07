---
title: Google Data Layer Extension
description: Adobe Experience PlatformのGoogle クライアントデータレイヤーのタグ拡張機能について説明します。
exl-id: 7990351d-8669-432b-94a9-4f9db1c2b3fe
source-git-commit: c61afdc2c3df98a0ef815d7cb034ba2907c52908
workflow-type: tm+mt
source-wordcount: '937'
ht-degree: 12%

---

# Google データレイヤーの拡張機能

Google データレイヤー拡張機能を使用すると、タグ実装でGoogle データレイヤーを使用できます。 この拡張機能は、個別に使用することも、Google ソリューションおよびGoogleのオープンソース [Data Layer Helper Library](https://github.com/google/data-layer-helper) と同時に使用することもできます。

ヘルパーライブラリは、Adobeクライアントデータレイヤー（ACDL）と同様のイベント駆動機能を提供します。 Google データレイヤー拡張機能のデータ要素、ルール、アクションは、[ACDL 拡張機能 ](../client-data-layer/overview.md) のものと同様の機能を提供します。

## インストール

拡張機能をインストールするには、データ収集 UI の拡張機能カタログに移動し、「**[!UICONTROL Google Data Layer]**」を選択します。

インストールが完了すると、Adobe Experience Platform タグライブラリのすべての読み込み時にデータレイヤーが作成されるか、データレイヤーにアクセスします。

## 拡張機能の表示

拡張機能の設定を使用して、拡張機能が使用するデータレイヤーの名前を定義できます。 Adobe Experience Platform タグの読み込み時に、設定された名前を持つデータレイヤーが存在しない場合、拡張子によって作成されます。

データレイヤー名のデフォルトは、Googleのデフォルト名 `dataLayer` です。

>[!NOTE]
>
>GoogleとAdobeコードのどちらを最初に読み込んでデータレイヤーを作成するかは関係ありません。 どちらのシステムも同じように動作します。データレイヤーが存在しない場合は作成し、既存のデータレイヤーを使用します。

## イベント

>[!NOTE]
>
>イベント駆動型のデータレイヤーをAdobe Experience Platform タグで使用すると、_event_ という単語がオーバーロードされる。 _イベント_ は次になることができます。
> - Adobe Experience Platform タグイベント（ライブラリの読み込みなど）。
> - JavaScript イベント。
> - _event_ キーワードを使用してデータレイヤーにプッシュされたデータ。

拡張機能を使用すると、データレイヤー上の変更をリッスンできます。

>[!NOTE]
>
>Adobeのクライアントデータレイヤーと同様に、データがGoogle データレイヤーにプッシュされる際の _event_ キーワードの使用を理解することが重要です。 _event_ キーワードは、Google データレイヤーの動作を変更するので、この拡張機能も変更されます。\
> 詳しくは、Googleのドキュメントを参照するか、不明な場合は調査を行ってください。

### Google イベントタイプ

Googleでは、イベントをプッシュする手段として、`push()` メソッドを使用するGoogle Tag Manager と、`gtag()` メソッドを使用するGoogle Analytics 4 の 2 つをサポートしています。

1.2.1 より前のバージョンのGoogle データレイヤー拡張機能では、このページのコード例に示すように、`push()` で作成されたイベントのみをサポートしていました。

バージョン 1.2.1 以降では、`gtag()` を使用して作成されたイベントがサポートされています。  これはオプションであり、拡張機能の設定ダイアログで有効にすることができます。

`push()` イベントと `gtag()` イベントについて詳しくは、[Google ドキュメント ](https://developers.google.com/analytics/devguides/collection/ga4/reference/events?client_type=gtag) を参照してください。  この情報は、拡張機能の設定ダイアログとルールダイアログでも提供されます。

### データレイヤーへのすべてのプッシュをリッスン

このオプションを選択すると、イベントリスナーはこのデータレイヤーに加えられた変更をリッスンします。

### イベントを除くプッシュをリッスン

このオプションを選択すると、イベントリスナーは、イベントを除く、データレイヤーへのデータのプッシュをリッスンします。

次のプッシュイベントの例は、リスナーによってトラッキングされます。

```js
dataLayer.push({"data":"something"})
```

次のプッシュイベントの例は、リスナーではトラッキングされません。

```js
dataLayer.push({"event":"myevent"})
dataLayer.push({"event":"myevent","data":"something"})
```

### すべてのイベントをリッスン

このオプションを選択すると、イベントリスナーは、データレイヤーにプッシュされたすべてのイベントをリッスンします。

次のプッシュイベントの例は、リスナーによってトラッキングされます。

```js
dataLayer.push({"event":"myevent"})
dataLayer.push({"event":"myevent","data":"something"})
```

次のプッシュイベントの例は、リスナーによってトラッキングされません。

```js
dataLayer.push({"data":"something"})
```

### 特定のイベントをリッスン

イベントを指定すると、イベントリスナーで特定の文字列に一致するイベントをトラッキングできます。

例えば、`myEvent` を指定してこの設定を使用すると、リスナーは次のプッシュイベントのみをトラッキングします。

```js
dataLayer.push({"event":"myEvent"})
```

（ECMAScript/JavaScript）正規表現を使用して、イベント名を一致させることができます。

例えば、「myEvent\d」と設定すると、`myEvent` が数字（\d）で追跡されます。

```js
dataLayer.push({"event":"myEvent1"})
dataLayer.push({"event":"myEvent2"})
```

## アクション

### データレイヤーにプッシュ {#push-to-data-layer}

この拡張機能では、JSON をデータレイヤーにプッシュする 2 つのアクションが提供されます。プッシュする JSON を手動で作成するフリーテキストフィールドと、バージョン 1.2.0 から、キー値マルチフィールドダイアログです。

#### フリーテキスト JSON

フリーテキストアクションを使用すると、JSON 内で直接データ要素を使用できます。 JSON エディター内では、データ要素はパーセント表記を使用して参照する必要があります。 例：`%dataElementName%`。

```json
{
  "page": {
    "url": "%url%",
    "previous_url": "%previous_url%",
    "concatenated_values": "static string %dataElement%"
  }
}
```

#### キー値マルチフィールド

新しいキー値マルチフィールドダイアログは、JSON を手動で書かなくてもプッシュを設定できる、より使いやすいインターフェイスです。

### Google DL が計算済み状態にリセットされました

拡張機能には、データレイヤーをリセットするアクションが用意されています。 Google データレイヤーの変更を処理するルールで使用すると、データレイヤーは、ルールがトリガーされた時点のデータレイヤーの計算済みステートにリセットされます。 Google データレイヤーの変更を処理しないルールでアクションが使用された場合、そのアクションはデータレイヤーを空にします。

## データ要素

指定されたデータ要素は、Google データレイヤーの変更（プッシュイベント）によってトリガーされたルールの実行中、またはライブラリの読み込みなどの無関係なルールで使用できます。 前者の場合、データ要素は、データレイヤーの変更時に計算された状態から取得した値を返します。 後者の場合は、ルール実行時に計算された状態が使用されます。

トグルスイッチを使用すると、データ要素が計算済み状態全体の値を返すか、イベント情報のみから値を返すかを選択できます（データレイヤーの変更によってトリガーされるルールで使用される場合）。

したがって、データ要素は次を返す可能性があります。

- 空のフィールド : データレイヤーの計算済み状態。
- キーを含むフィールド（上記の例では page.previous_url など）：イベントオブジェクトまたは計算済み状態のキーの値。

## 追加情報

拡張機能のデータ要素ダイアログとイベントダイアログには、詳細な使用情報と例が含まれています。

その他の一般情報は、[project README](https://github.com/adobe/reactor-extension-googledatalayer/blob/main/README.md) にあります
