---
title: Adobe Experience PlatformWeb SDK Extensionリリースノート
description: Adobe Experience Platform Launch の Adobe Experience Platform Web SDK 拡張機能
seo-description: Adobe Experience Platform Launch の Adobe Experience Platform Web SDK 拡張機能
translation-type: tm+mt
source-git-commit: 14cf62084c88956906cd9454176619ed08081a0e
workflow-type: tm+mt
source-wordcount: '1134'
ht-degree: 79%

---


# Adobe Experience PlatformWeb SDK拡張機能リリースノート

このドキュメントでは、Adobe Experience Platform Launch向けAdobe Experience PlatformWeb SDK extensionのリリースノートをカバーしています。 SDK自体の最新のリリースノートについては、[プラットフォームWeb SDKリリースノート](https://docs.adobe.com/content/help/ja-JP/experience-platform/edge/release-notes.html)を参照してください。

## 2020 年 3 月 9 日

### Adobe Experience Platform Web SDK 2.4.0

Adobe Experience Platform Web SDK ライブラリのバージョン 2.4.0 が含まれます。

* 「イベントアクションUIを送信」に、[「ドキュメントのアンロード中」](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/tracking-events.html?lang=en#using-the-sendbeacon-api)チェックボックスが追加されました。
* [デフォルトの同意](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html#default-consent)を設定すると、同意を受けるまですべてのイベントを破棄する`out`オプションのサポートを追加しました(既存の`pending`オプションは、同意を受けたらイベントをキューに入れ、送信します)。
* デフォルトの同意フィールドにツールチップを追加しました。
* [Adobeの同意2.0基準](https://experienceleague.adobe.com/docs/experience-platform/edge/consent/supporting-consent.html?communicating-consent-preferences-via-the-adobe-standard)のサポートを追加しました。
* ユーザーのアクセストークンが無効であるか、適切にプロビジョニングされていない場合に、XDMオブジェクトのデータ要素のUIにより適切なエラーが表示されるようになりました。
* XDMオブジェクトデータ要素を表示するとブラウザー開発者コンソールに表示される、接触チャネル間のエラー（拡張機能の動作には影響しません）を修正しました。

## 2020 年 11 月 4 日

### Adobe Experience Platform Web SDK 2.3.0

Adobe Experience Platform Web SDK ライブラリのバージョン 2.3.0 が含まれます。

#### 機能

* デフォルトの同意を設定する際のデータ要素の使用に関するサポートを追加しました。
* XDM オブジェクトデータ要素タイプで XDM スキーマを検索する機能を追加しました。
* XDM データオブジェクトに対する以降の変更が要求に反映されないように、「イベントの送信」アクションタイプ内に XDM データのクローンを追加しました。

## 2020 年 10 月 2 日

### Adobe Experience Platform Web SDK 2.2.0

#### バグの修正

* お客様がサンドボックススキーマから XDM オブジェクトを作成しようとすると、認証の問題が発生していました。Platformを呼び出すAPIは、環境を認識するようになったので、編集にアクセスできるスキーマのみが表示されます。

#### 機能

* `identityMap` データ要素を使用する場合、 名前空間はドロップダウンで事前設定されるので、手動で入力する必要がありません。
* `xdmObject` データ要素の UI が改良されました。新しい UI では、オブジェクトに各項目を入力しなくても、入力されたフィールドを確認できます。


## 2020 年 8 月 27 日

### Adobe Experience Platform Web SDK 2.1.1

#### 機能

* XDM オブジェクトビューの Adobe Experience Platform サンドボックスが正しく表示されない問題を修正しました。このバージョンの拡張機能を使用する場合、予期されたサンドボックスがリストに表示されない場合は、ユーザーは Adobe Experience Platform 管理者に問い合わせて、アクセス権限が正しく設定されていることを確認する必要があります。


## 2020 年 8 月 6 日

### Adobe Experience Platform Web SDK 2.1.0

#### 機能

* 重大な変更：`syncIdentity` アクションを削除し、代わりに `sendEvent` アクションにこれらの ID を渡すことをサポートします。拡張機能をアップグレードする前に、この操作を使用する既存のルールを無効にしてください。
* Ally v. 2.1.0 へのアップデート（[リリースノート](https://docs.adobe.com/content/help/en/experience-platform/edge/release-notes.html)）。
* `setConsent` アクションでの IAB 2.0 Consent Standard をサポートします。
* `sendEvent` アクションでのデータセット ID の上書きをサポートします。
* タイプ `IdentityMap` の新しいデータ要素を追加します。これは、現在有効になっている XDM オブジェクトデータ要素と `setConsent` アクションに `identityMap` エントリを入力するために使用できます。
* `setConsent` アクションで ID マップを渡す機能をサポートします。
* XDMオブジェクトデータ要素でのプラットフォームサンドボックスの選択をサポートします。


## 2020 年 5 月 27 日

### Adobe Experience Platform Web SDK 1.0.0

#### 機能

* Configuration Service からの環境の選択をサポートします。


## 2020 年 5 月 5 日

### Adobe Experience Platform Web SDK 0.1.2

#### 機能

* `configId` の名前を `edgeConfigId` に変更しました。
* `viewStart` の名前を `renderDecisions` に変更しました。デフォルトでは「false」に設定されています。「true」に設定すると、パーソナライゼーションオファーを取得して自動的的にレンダリングされます。
* `Get Decisions` に関する変更：
   * `getDecisions` コマンドを削除しました。
   * `sendEvent` コマンドに `scopes` オプションを追加しました。決定は、`sendEvent` によって解決されたプロミスで返されます。
   * 組み込みの `__view__` 範囲が追加され、ページ全体またはビュー全体のオファーを返すようになりました。（Target の VEC オファーなど）これらの決定は、`renderDecisions` が false に設定されている場合にのみ、`sendEvent` コマンドから返されます。
   * 決定を利用可能になったときに実行する `Decisions Received` イベントを追加しました。
* 1 回のサーバーコールで複数のパーソナライゼーション通知を組み合わせました。
* データ要素が参照されるたびにイベント結合 ID がリセットされる問題を修正しました。
* `setCustomerIds` アクションの名前を `syncIdentity` に変更しました。
* `getIdentity` コマンドを追加しました。現在のところ、カスタムコードでのみ使用できます。
* `_satellite`を使用してデバッグを有効にすると、Adobe Experience PlatformWeb SDKでのデバッグが有効になります。
* XDM オブジェクトに入力された値（ブール値、数字、および小数）のサポートを追加しました。

## 2020 年 3 月 17 日

### Adobe Experience Platform Web SDK 0.0.10

#### 機能

* `Consent` の下にオプトインとオプトアウトの概念を組み合わせ、`setConsent` コマンドを追加しました。
* JavaScript／JSON から XDM へのマッピングを可能にする、新しいタイプ `XDM Object` のデータ要素を追加しました。

## 2020 年 2 月 19 日

### Adobe Experience Platform Web SDK 0.0.7

#### 機能

* Removed idSyncContainerId、datasetId、schemaId、urlDestinationsEnabled、および cookieDestinationsEnabled オプションが削除されました。
* edgeDomain オプション値でのハイフンのサポートを追加しました。
* ID の移行中におこなわれるリクエストは、demdex cookie が設定されていない場合にクロスドメイン識別を改善するために demdex エンドポイントに送信されます。
* ID の移行中におこなわれるリクエストでは常に、ID cookie が設定されることを確認する応答を必要とします。
* 無効なコマンドを実行すると、有効なコマンド名のリストがコンソールに記録されます。
* サードパーティ cookie のサポートを Adobe Experience Platform Launch 拡張機能に切り替えるためのチェックボックスが追加されました。これにより、demdex.net への呼び出しが無効になります。

## 2019 年 12 月 21 日

### Adobe Experience Platform Web SDK 0.0.5

#### 機能

* アクティビティトラッカーの設定を Platform Launch 拡張機能に追加
* イベントコマンドで EventType と EventMergeId を公開。
* Platform Launch 拡張機能に onBeforeEventSend 設定を追加
* Platform Launch 拡張機能に edgeBasePath 設定を追加

#### 次の変更を含む Alloy v. 0.0.10 への更新：

* クライアントストレージの実装：状態と cookie のロジックがサーバーに移動されました。
* イベントコマンドで EventType と EventMergeId を公開。
* 離脱リンク以外のリンクトラッキングに sendBeacon を使用する。
* ID 同期から有効期限のチェックを引いた値を戻す。
* 非 SSL（http）ページで setCustomerIds コマンドによって ID がハッシュされない。
* 状態 / cookie を設定する際に使用するサーバーに APEX ドメインを渡す。
* 新しいハンドルタイプを使用して応答から ECID を取得。
* アクティベーションと ID の設定のデフォルトを削除。
* 名前を変更してクエリオプションをメタに移動。
* レガシー ECID の移行。

#### バグの修正

* 予期しない状態コードが発生した場合、エラーメッセージの応答本文を解析して形式を設定します。
* debug コマンドを実行するか alloy_debug を使用すると、設定によって上書きされます。

## 2019 年 11 月 26 日

### Adobe Experience Platform Web SDK 0.0.3

#### 機能

* 「Send Event」アクションの新しい「Merge ID」フィールドと「Type」フィールド。「Merge ID」は XDM スキーマ内の `xdm.eventMergeID` に、「Type」は XDM スキーマ内の `xdm.eventType` にマッピングします。
* エラー処理とレポートの改善。
* すべてのリンクで `sendBeacon` を使用。

#### バグの修正

* クエリ文字列パラメーターを使用してデバッグを切り替えたり、`debug` コマンドがセッション中持続しない問題を修正しました。

## 2019 年 11 月 19 日

### Adobe Experience Platform Web SDK 0.0.2

#### 機能

* 拡張機能を追加。
* 追加のライブラリやネットワーク呼び出しが不要な ECID サポート。
* オプトインのサポート。
* XDMをプラットフォームに送信するサポート
* ファーストパーティドメインのサポート。
* ブラウザーコンテキストを自動的に収集。
* 完全なオープンソース（[拡張機能](https://github.com/adobe/reactor-extension-alloy)、[SDK](https://github.com/adobe/reactor-extension-alloy)）。
* 詳細なログ。
* 実稼動環境でのエラーの非表示機能。
