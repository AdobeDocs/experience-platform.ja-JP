---
title: Adobe Analytics 拡張機能のリリースノート
description: Adobe Experience Platform の Adobe Analytics タグ拡張機能に関する最新のリリースノートです。
exl-id: 3c7b4ec0-4b81-4ef4-b15f-6ad102525840
source-git-commit: 5f4e157a39bf927b3821931d55f968862b2ed16d
workflow-type: tm+mt
source-wordcount: '1524'
ht-degree: 70%

---

# Adobe Analytics 拡張機能のリリースノート

以下は、Adobe Analytics タグ拡張機能のリリースノートのリストです。

>[!NOTE]
>
>[AppMeasurementのJavaScript ライブラリ ](https://experienceleague.adobe.com/docs/analytics/implementation/js/overview.html?lang=ja) の更新に対応して Analytics タグ拡張機能が頻繁に更新される場合。 以下に説明する具体的なバージョンについて詳しくは [&#128279;](https://experienceleague.adobe.com/docs/analytics/implementation/appmeasurement-updates.html?lang=ja)AppMeasurementのリリースノート &rbrace; を参照してください。

## 2024 年 10 月 28 日（Pt）

**Adobe Analytics Extension 1.9.6**

**機能**:

* ユーザーが [ 変数を設定アクション ](https://experienceleague.adobe.com/en/docs/experience-platform/tags/extensions/client/analytics/overview#set-variables) の JSON バージョンを表示および編集できる新機能を追加しました。 Adobe Web SDK拡張機能には、JSON を提供することで [Analytics 変数への入力 ](https://experienceleague.adobe.com/en/docs/experience-platform/tags/extensions/client/web-sdk/data-element-types) を行うアクションも含まれています。 JSON データを AA 拡張機能から Web SDK拡張機能にコピーすると、移行するお客様は各変数を手動で追加する代わりに、一度に複数の設定を簡単に転送できます。

## 2024 年 8 月 12 日（Pt）

**Adobe Analytics Extension 1.9.5**

**機能**:

* [AppMeasurementに v2.27.0](https://github.com/adobe/appmeasurement/releases/tag/v2.27.0) にアップグレードしました。

## 2024 年 3 月 4 日（Pt）

**Adobe Analytics Extension 1.9.4**

**機能**:

* [AppMeasurementに v2.26.0](https://github.com/adobe/appmeasurement/releases/tag/v2.26.0) にアップグレードしました。

## 2023 年 9 月 15 日（Pt）

**Adobe Analytics Extension 1.9.3**

**機能**:

* [AppMeasurementに v2.25.0](https://github.com/adobe/appmeasurement/releases/tag/v2.25.0) にアップグレードしました。


## 2023 年 7 月 19 日（Pt）

**Adobe Analytics Extension 1.9.2**

**機能**:

* [AppMeasurement v2.24.0](https://github.com/adobe/appmeasurement/releases/tag/v2.24.0) にアップグレードしました。
* 2 バイトのエンコードされた文字 `decodeLinkParameters` 含むリンク URL をデコードするオプションの設定（デフォルト `false`）を追加しました。

**バグ修正**:

* 高エントロピー [User-Agent クライアントヒント ](https://experienceleague.adobe.com/docs/analytics/technotes/client-hints.html) API の不具合があるブラウザー向けに、エラー処理を追加しました。
* [Header](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Methods/POST) Content-Type POSTが `x-www-form-urlencoded` を既定で使用するように変更されました。

## 2022 年 9 月 23 日（Pt）

**Adobe Analytics Extension 1.9.1**

**機能**:

* AppMeasurement v2.23.0 にアップグレードしました。
* 最新バージョンのAppMeasurementでサポートされているように、拡張機能は高エントロピー [user-agent クライアントヒント ](https://developer.mozilla.org/en-US/docs/Web/HTTP/Client_hints#user-agent_client_hints) を収集できるようになりました。

## 2022 年 2 月 28 日（Pt）

**Adobe Analytics Extension 1.9.0**

**バグ修正**:

* AppMeasurementから一部の debug ステートメントを削除しました。

## 2021年11月29日（PT）

**Adobe Analytics Extension 1.8.8**

**バグ修正**:

* AppMeasurementを v2.22.3 にアップグレードしました。

## 2021年9月16日（PT）

**Adobe Analytics Extension 1.8.7**

**バグ修正**:

* AppMeasurement を v2.22.2 にアップグレードしました。
* 非推奨（廃止予定）の buildInfo.environment を削除

## 2021 年 8 月 24 日（PT）

**Adobe Analytics Extension 1.8.6**

**バグ修正**:

* [AppMeasurement を v2.22.1](https://experienceleague.adobe.com/docs/analytics/implementation/appmeasurement-updates.html?lang=ja) にアップグレードしました。
* innerHTML を使用する代わりに、Activity Map ロジックを反映するように linkName のフォールバックを更新しました。

## 2020 年 8 月 6 日（PT）

**Adobe Analytics Extension 1.8.5**

**バグ修正**:

* このフィールドを空白のままにした場合に、AAM モジュール設定で誤った Cookie 名が設定されていました。これは修正されました。

**機能**:

* [AppMeasurement を 2.22.0](https://experienceleague.adobe.com/docs/analytics/implementation/appmeasurement-updates.html?lang=ja) に更新しました。
* 小さな UI が変更され、追加の設定がチェックボックスではなくアコーディオンで折りたたまれて表示されるようになりました。

## 2020 年 6 月 2 日（PT）

**Adobe Analytics Extension 1.8.4**

**バグ修正**:

* 買い物かごのイベント（prodView、scAdd、scView など）がイベントドロップダウンに表示されない問題を修正しました。これらすべてをドロップダウンから選択できるようになります。

**機能**:

* カスタムコードを使用しなくても、拡張機能の Activity Map をオフにできるようになりました。Activity Map は、AAM モジュールと同様に、個別のモジュールとして読み込まれ、必要に応じてオフにすることができます。
* 階層変数やその他のオプションを最小限に抑えて UI を整理しました。
* 拡張機能設定 UI から購入 ID を設定するためのフィールドを追加しました。

## 2020 年 3 月 10 日（PT）

**Adobe Analytics Extension 1.8.3**

**バグ修正**:

* カスタムライブラリを使用しており、レポートスイートが Analytics で設定されていない場合、変数を設定しようとするとエラーが発生する、ルール設定に影響するバグを修正しました。
* eVar を作成する際、「複製元」を prop とするオプションまたは prop に複製するオプションを表示しないバグが発生していました。このバグが修正され、以前のバージョンの動作を反映するようになりました。

**機能**:

* [AppMeasurement を 2.20.0](https://experienceleague.adobe.com/docs/analytics/implementation/appmeasurement-updates.html?lang=ja) に更新しました

## 2020 年 3 月 2 日（PT）

**Adobe Analytics Extension 1.8.2**

**バグ修正**:

* 数値イベントやシリアル化された通貨で間違った構文が使用されていた問題を修正しました。

**機能**:

* [AppMeasurement を 2.18.0](https://experienceleague.adobe.com/docs/analytics/implementation/appmeasurement-updates.html?lang=ja) に更新しました
* Audience Manager モジュールの DIL ライブラリを 9.4 に更新しました
* 拡張機能内の入力フィールドの長さを増やしました
* 拡張機能およびアクション設定の eVar と prop で、Analytics のわかりやすい名前が表示されるようになりました
* 拡張機能設定の「Cookie」セクションに、セキュリティで保護された cookie を書き込むためのチェックボックスを追加しました。
* Audience Manager モジュールに 3 つの新しい設定を追加しました。ログを有効にする、URL の宛先を有効にする、Cookie の宛先を有効にする設定を追加しました。

## 2019 年 11 月 13 日（PT）

**Adobe Analytics Extension 1.8.1**

**バグ修正**:

* プレミアムの eVar と prop が保存されないバグを修正しました。

## 2019 年 11 月 1 日（PT）

**Adobe Analytics Extension 1.8.0**

**バグ修正**:

* 少数の顧客が、ドロップダウンにレポートスイートのオプションを表示できないというバグを修正しました。
* ECID を使用している間、一部の変数が正しく設定されない問題を修正しました。

**機能**:

* 拡張機能ビューの eVar、prop およびイベントを数値順に並べ替えます
* Magento コンテキストデータをサポートするためにバックエンドスキーマを変更しました

## 2019 年 9 月 6 日（PT）

**Adobe Analytics Extension 1.7.8**

**バグ修正**:

* ドロップダウンのレポートスイートオプションが、一部のユーザーに表示されない表示しないバグを修正しました。
* イベントが正しく実行されないバグを修正しました。

## 2019 年 9 月 5 日（PT）

**Adobe Analytics Extension 1.7.7**

**機能**:

* AppMeasurement が 2.17 に更新されました
* Audience Management モジュールを DIL 9.3 をサポートするよう更新されました。
* スペースを増やすよう、フィールドの幅を更新しました。

**バグ修正**:

* オプトイン／オプトアウトの設定に関するバグを修正しました。
* ECID を使用する際に変数が正しく設定されないバグを修正しました。

## 2019 年 7 月 18 日（PT）

**Adobe Analytics Extension 1.7.6**

**機能**:

* DIL 9.2 for Audience Manager をサポートするように Adobe Analytics 拡張機能を更新しました

* [AppMeasurement 2.15.0](https://experienceleague.adobe.com/docs/analytics/implementation/appmeasurement-updates.html#version-2.15.0) をサポートするように拡張機能を更新しました
* サポートされなくなったので、次のチェックボックスを削除しました：「宛先を公開する IFRAME を DOM または Fire の宛先に添付しない」

## 2019 年 6 月 4 日（PT）

**Adobe Analytics Extension 1.7.5**

**機能**:

* Adobe Analytics 拡張機能を、既知の clearVars の問題を修正する [AppMeasurement 2.14.0](https://experienceleague.adobe.com/docs/analytics/implementation/appmeasurement-updates.html#version-2.14.0)に更新しました
* 拡張機能への Exchange リンクを追加しました。ドロップダウンリストを選択して「拡張機能情報」を選択すると、Exchange リストにアクセスできます。

**バグ修正**:

* 誤った evar がリストから削除されるという UI のバグを修正しました
* 複数のレポートスイートを追加しようとすると SSL トラッキングサーバーが必要となるバグを修正しました。複数のレポートスイートを追加する場合、トラッキングサーバーは必要ですが、SSL トラッキングサーバーフィールドはオプションです。

## 2019 年 4 月 15 日（PT）

**Adobe Analytics Extension 1.7.4**

**バグ修正**:

* AppMeasurement 2.13.0 でバグが見つかった後に拡張機能をロールバックしました。AppMeasurement 2.13.0 では、ECID に送信されない問題が発生していました。そのため、1.7.3 をインストールしている場合は、問題を回避するため、1.7.4 にアップグレードすることをお勧めします。AppMeasurement の更新バージョンがリリースされるまで、clearVars は継続します

## 2019 年 4 月 12 日（PT）

**Adobe Analytics Extension 1.7.3**

**バグ修正**:

* Adobe Analytics 拡張機能を、既知の clearVars の問題を修正する AppMeasurement 2.13.0 に更新しました。

## 2019 年 3 月 21 日（PT）

**Adobe Analytics Extension 1.7.2**

**機能**:

* Adobe Analytics拡張機能を DIL 9.1 に更新しました。
* Adobe Analytics 拡張機能を AppMeasurement 2.12 に更新しました。
* Adobe Analytics 拡張機能ビューを React-Spectrum にアップグレードしました。
* 設定ページでレポートスイートを設定すると、社内のすべてのレポートスイートにドロップダウンが表示され、適切なるレポートスイートを簡単に選択できるようになりました。

## 2019 年 3 月 7 日（PT）

**Adobe Analytics Extension 1.7.1**

**バグ修正**:

* 1.7 でバグが検出された後、拡張機能をバージョン 1.6 に戻しました。

## 2019 年 2 月 11 日（PT）

**Adobe Analytics Extension 1.6**

**機能**:

* Adobe Analytics 拡張機能が、オプトインをサポートする DIL 9.0 に更新されました。
* Adobe Analytics 拡張機能が、オプトインをサポートす AppMeasurement 2.11 に更新されました。

**バグ修正**:

* Prototype JS との競合を修正しました。Analytics 拡張機能は、標準的な prototype.js ライブラリをサポートするようになります。

## 2018 年 11 月 9 日（PT）

**Adobe Analytics Extension 1.5.1**

**バグ修正**:

* DIL モジュールを 7.0 にダウングレードして、解析ビーコンが実行されなかった問題を修正しました

## 2018 年 11 月 5 日（PT）

**Adobe Analytics Extension 1.5**

**機能**:

* Audience Manager で DIL 8.0 をサポートするように Adobe Analytics 拡張機能を更新しました
* 「Serialize from value」フィールドを、「Event ID」と「Event Value」の 2 つに分割しました。これにより、イベントがシリアル化されず、値を割り当てていた問題が修正されます。
   * 注：文字列（例：Event7=3:abc123）を使用して ID を追加する際、現在のフィールドを使用している場合は、「Event ID」フィールドの ID を反映するよう入力内容を更新する必要があります。

**バグ修正**:

* 通貨コードが正しく入力できなかったバグを修正しました

## 2018 年 10 月 11 日（PT）

**Adobe Analytics Extension 1.4**

**機能**:

* トラッキング cookie 名を拡張機能設定に移行しました。

**バグ修正**:

* trackerProperties オブジェクトがない場合に、変数がクラッシュすることがないように、バグを修正しました。

## 2018 年 6 月 5 日（PT）

**Adobe Analytics Extension 1.3**

**機能**:

* AppMeasurement 2.9 をサポートするように Adobe Analytics 拡張機能を更新しました。
* Adobe Analytics拡張機能に「トラッカーをグローバルにアクセスできるようにする」機能を追加しました。これにより、`windows.s` の下でトラッカーをグローバルにスコープ設定できます。

**バグ修正**:

* 詳細ビューから戻るとリスト表示がリセットされるバグを修正しました
* いくつかのバグを修正し、リビジョンセレクターでリソースの読み込みを改善しました
* 複数のルールが Adobe Analytics 拡張機能の s.events を上書きしていたバグを修正しました

## 2018 年 3 月 20 日（PT）

**Adobe Analytics Extension 1.2**

**機能**:

* AppMeasurement. js を 2.8.0 に更新しました
* サーバー側転送のサポートを追加しました

## 2018 年 2 月 8 日（PT）

**Adobe Analytics Extension 1.1**

**機能**:

* AppMeasurement はバージョン 2.6 に更新されました
* 初期化された Analytics トラッカーが Adobe Experience Platform タグ拡張機能の共有モジュールを使用して公開されるようになりました。これにより、やり取りするコードを他の拡張機能に含めることができます。

**バグ修正**:

* Adobe Analytics 拡張機能で、「Error, missing Report Suite ID in AppMeasurement initialization」がブラウザーコンソールに表示されていたエラーを修正しました。
