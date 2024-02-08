---
title: Google Data Layer 拡張機能
description: Adobe Experience PlatformのGoogle Client Data Layer タグ拡張機能について説明します。
exl-id: 7990351d-8669-432b-94a9-4f9db1c2b3fe
source-git-commit: c61afdc2c3df98a0ef815d7cb034ba2907c52908
workflow-type: tm+mt
source-wordcount: '937'
ht-degree: 12%

---

# Google Data Layer 拡張機能

Google Data Layer 拡張機能を使用すると、タグ実装でGoogleデータレイヤーを使用できます。 この拡張機能は、単独で、またはGoogleソリューションと同時に、Googleのオープンソースと共に使用できます [データレイヤーヘルパーライブラリ](https://github.com/google/data-layer-helper).

ヘルパーライブラリは、イベントクライアントデータレイヤー (ACDL) と同様のAdobe駆動型機能を提供します。 Google Data Layer 拡張機能のデータ要素、ルールおよびアクションは、 [ACDL 拡張機能](../client-data-layer/overview.md).

## インストール

拡張機能をインストールするには、データ収集 UI の拡張機能カタログに移動し、「 」を選択します。 **[!UICONTROL Google Data Layer]**.

インストールすると、この拡張機能は、Adobe Experience Platform Tags ライブラリの読み込みのたびに、データレイヤーを作成するか、データレイヤーにアクセスします。

## 拡張機能ビュー

拡張機能の設定を使用して、拡張機能が使用するデータレイヤーの名前を定義できます。 Adobe Experience Platformタグの読み込み時に設定済みの名前のデータレイヤーが存在しない場合、拡張機能によって作成されます。

データレイヤー名のデフォルトは、Googleのデフォルト名です。 `dataLayer`.

>[!NOTE]
>
>GoogleまたはAdobeコードが最初に読み込まれて、データレイヤーが作成されるかどうかは関係ありません。 両方のシステムが同じように動作します。データレイヤーが存在しない場合や既存のデータレイヤーを使用する場合は、データレイヤーを作成します。

## イベント

>[!NOTE]
>
>次の単語 _イベント_ イベント駆動型データレイヤーがAdobe Experience Platformタグで使用されている場合はオーバーロードされます。 _イベント_ は次のいずれかになります。
> - Adobe Experience Platform Tags イベント（「Library Loaded」など）。
> - JavaScript イベント。
> - を使用してデータレイヤーにプッシュされたデータ _イベント_ キーワード。

拡張機能を使用すると、データレイヤーの変更をリッスンできます。

>[!NOTE]
>
>この _イベント_ キーワード。データがGoogleデータレイヤーに (Adobeクライアントデータレイヤーと同様に ) プッシュされたときに使用します。 The _イベント_ キーワードは、Googleデータレイヤー（したがってこの拡張）の動作を変更します。\
> この点について不明な点がある場合は、 Googleのドキュメントを読むか、調査してください。

### Google Event Types

Googleは、次の 2 つのプッシュ方法をサポートしています。 Google Tag Manager は、 `push()` メソッドとGoogle Analytics4、を使用 `gtag()` メソッド。

1.2.1 より前のGoogle Data Layer 拡張機能バージョンでは、 `push()`に含めることができます。

バージョン 1.2.1 以降では、 `gtag()`.  これはオプションで、拡張機能設定ダイアログで有効にできます。

詳しくは、 `push()` および `gtag()` イベントについては、 [Googleドキュメント](https://developers.google.com/analytics/devguides/collection/ga4/reference/events?client_type=gtag).  情報は、拡張機能の設定ダイアログとルールダイアログでも提供されます。

### データレイヤーへのすべてのプッシュをリッスンします。

このオプションを選択すると、イベントリスナーはこのデータレイヤーに加えられた変更をリッスンします。

### イベントを除くプッシュのリッスン

このオプションを選択した場合、イベントリスナーは、データレイヤーへのデータのプッシュ（イベントを除く）をリッスンします。

次のプッシュイベントの例は、リスナーによってトラッキングされます。

```js
dataLayer.push({"data":"something"})
```

次のサンプルのプッシュイベントは、リスナーによって追跡されません。

```js
dataLayer.push({"event":"myevent"})
dataLayer.push({"event":"myevent","data":"something"})
```

### すべてのイベントをリッスンする

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

### 特定のイベントをリッスンする

イベントを指定すると、イベントリスナーで特定の文字列に一致するイベントをトラッキングできます。

例えば、`myEvent` を指定してこの設定を使用すると、リスナーは次のプッシュイベントのみをトラッキングします。

```js
dataLayer.push({"event":"myEvent"})
```

(ECMAScript/JavaScript) 正規表現を使用して、イベント名を照合できます。

例えば、&#39;myEvent\d&#39;を設定すると、 `myEvent` 数字 (\d):

```js
dataLayer.push({"event":"myEvent1"})
dataLayer.push({"event":"myEvent2"})
```

## アクション

### データレイヤーにプッシュ {#push-to-data-layer}

拡張機能では、JSON をデータレイヤーにプッシュする 2 つのアクションを提供します。プッシュする JSON を手動で作成する自由テキストフィールドと、キーと値のペアのマルチフィールドダイアログのバージョン 1.2.0 です。

#### フリーテキスト JSON

フリーテキストアクションを使用すると、JSON 内で直接データ要素を使用できます。 JSON エディター内では、データ要素は、パーセント表記を使用して参照する必要があります。 例：`%dataElementName%`。

```json
{
  "page": {
    "url": "%url%",
    "previous_url": "%previous_url%",
    "concatenated_values": "static string %dataElement%"
  }
}
```

#### キーと値のマルチフィールド

新しいキーと値のペアのマルチフィールドダイアログは、JSON を手動で書かずにプッシュを設定できる、より使いやすいインターフェイスです。

### Google DL の計算済み状態へのリセット

拡張機能では、データレイヤーをリセットするアクションを提供します。 Googleのデータレイヤーの変更を処理するルールで使用すると、データレイヤーは、ルールがトリガーされた時点で、データレイヤーの計算済み状態にリセットされます。 Googleのデータレイヤーの変更を処理しないルールでアクションが使用される場合、このアクションはデータレイヤーを空にします。

## データ要素

提供されたデータ要素は、Googleデータレイヤーの変更（プッシュイベント）によってトリガーされるルールの実行中に、または Library Loaded などの関連のないルールで使用できます。 前者の場合、データ要素は、データレイヤーの変更時に計算された状態から取得された値を返します。 後者の場合は、ルール実行時の計算済み状態が使用されます。

切り替えスイッチを使用すると、データ要素が計算済み状態全体から値を返すか、イベント情報（データレイヤーの変更によってトリガーされるルール内で使用される場合）から値を返すかを選択できます。

したがって、データ要素は次の値を返すことができます。

- 空のフィールド：データレイヤーの計算済み状態。
- キーを持つフィールド（上の例では page.previous_url など）：イベントオブジェクト内のキーの値または計算済み状態。

## 追加情報

拡張機能のデータ要素ダイアログとイベントダイアログには、使用に関する詳細な情報と例が含まれています。

その他の一般情報については、 [project README](https://github.com/adobe/reactor-extension-googledatalayer/blob/main/README.md)
