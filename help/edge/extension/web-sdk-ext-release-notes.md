---
title: Adobe Experience Platform Web SDK拡張機能のリリースノート
description: Adobe Experience Platform Web SDKタグ拡張
exl-id: 91de8c91-023a-45b6-9f67-ac75ee471e50
source-git-commit: a4a41a91429104b302e223034bf15f9839ddb5ad
workflow-type: tm+mt
source-wordcount: '1185'
ht-degree: 49%

---

# Adobe Experience Platform Web SDK拡張機能のリリースノート

このドキュメントでは、 Adobe Experience Platform Web SDKタグ拡張のリリースノートについて説明します。 SDK自体の最新のリリースノートについては、[Platform Web SDKリリースノート](https://experienceleague.adobe.com/docs/experience-platform/edge/release-notes.html)を参照してください。

## バージョン2.7.1 - 2021年9月8日

Adobe Experience Platform Web SDK ライブラリのバージョン 2.6.4 が含まれます。

## バージョン2.7.0 - 2021年8月17日

Adobe Experience Platform Web SDK ライブラリのバージョン 2.6.3 が含まれます。

* IDマップデータ要素タイプを使用する場合、文字列が設定されていない値に解決されるIDを持つIDは、IDマップから自動的に削除されるようになりました。
* XDMオブジェクトデータ要素タイプを使用してスキーマが選択されていないデータ要素を保存しようとすると発生するエラーを修正しました。
* ユーザーインターフェイスタイポグラフィを改善しました。

## バージョン2.6.2 - 2021年8月5日

Adobe Experience Platform Web SDK ライブラリのバージョン 2.6.2 が含まれます。

## バージョン2.6.1 - 2021年7月30日

Adobe Experience Platform Web SDK ライブラリのバージョン 2.6.1 が含まれます。

## バージョン2.6.0 - 2021年7月28日

Adobe Experience Platform Web SDK ライブラリのバージョン 2.6.0 が含まれます。

* 「エッジ設定」という用語を使用するラベル、説明およびエラーメッセージは、最新のAdobe Experience Platformの用語に合わせて「datastream」という用語を使用するように変更されました。
* 拡張機能の設定表示で、大量のデータストリームおよびデータストリーム環境を処理するためのサポートが追加されました。
* XDMオブジェクトデータ要素ビューで、多数のスキーマを処理するためのサポートが追加されました。
* Send Event Completeイベントタイプが追加されました。このイベントタイプは、イベントがサーバーに送信され、応答が受信された後にルールを実行するために使用できます。 近日中に、その他のドキュメントも提供される予定です。
* Decisions Receivedイベントタイプは非推奨（廃止予定）となりました。 代わりに、「 Send Event Complete 」イベントタイプを使用してください。
* ユーザーインターフェイスとエラー処理が一般的に改善されました。

## バージョン2.5.0 - 2021年6月2日

Adobe Experience Platform Web SDK ライブラリのバージョン 2.5.0 が含まれます。

* 「イベントを送信」アクションに`data`フィールドを追加しました。 今後のドキュメントでは、特定のシナリオでこの機能を使用する方法を説明します。
* XDMオブジェクトデータ要素ビューで、ユーザーがAdobe Experience Platformサンドボックスにアクセスできるが、組織のデフォルトとして設定されたサンドボックスにアクセスできない場合にエラーがスローされる問題が修正されました。
* XDMオブジェクトデータ要素ビューで、親オブジェクトに値が含まれていない場合でも、必須のスキーマフィールドが無効と見なされる問題が修正されました。

## バージョン2.4.0 - 2021年3月10日

Adobe Experience Platform Web SDK ライブラリのバージョン 2.4.0 が含まれます。

* 「イベント送信」アクションUIに[「ドキュメントのアンロード」](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/tracking-events.html?lang=en#using-the-sendbeacon-api)チェックボックスを追加しました。
* [同意を受けるまですべてのイベントを破棄するデフォルトの同意](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html#default-consent)を設定する際の`out`オプションのサポートを追加しました（既存の`pending`オプションはイベントをキューに追加し、同意を得た後に送信します）。
* デフォルトの同意フィールドにツールチップを追加しました。
* [Adobeの同意2.0標準](https://experienceleague.adobe.com/docs/experience-platform/edge/consent/supporting-consent.html?communicating-consent-preferences-via-the-adobe-standard)のサポートを追加しました。
* ユーザーのアクセストークンが無効か不適切にプロビジョニングされた場合、XDMオブジェクトデータ要素のUIに、より良いエラーが表示されるようになりました。
* XDMオブジェクトデータ要素を表示した際にブラウザー開発者コンソールに表示されるクロスオリジンエラー（拡張機能の操作には影響しません）を修正しました。

## バージョン2.3.0 - 2020年11月5日

Adobe Experience Platform Web SDK ライブラリのバージョン 2.3.0 が含まれます。

* デフォルトの同意を設定する際のデータ要素の使用に関するサポートを追加しました。
* XDM オブジェクトデータ要素タイプで XDM スキーマを検索する機能を追加しました。
* XDM データオブジェクトに対する以降の変更が要求に反映されないように、「イベントの送信」アクションタイプ内に XDM データのクローンを追加しました。

## バージョン2.2.0 - 2020年10月2日

* お客様がサンドボックススキーマから XDM オブジェクトを作成しようとすると、認証の問題が発生していました。Platformを呼び出すAPIは環境を認識するようになったので、ユーザーには、編集にアクセスできるスキーマのみが表示されます。
* `identityMap`データ要素を使用する場合、名前空間がドロップダウンに事前入力されるので、手動で入力する必要がなくなりました。
* `xdmObject` データ要素の UI が改良されました。新しい UI では、オブジェクトに各項目を入力しなくても、入力されたフィールドを確認できます。

## バージョン2.1.1 - 2020年8月27日

* XDM オブジェクトビューの Adobe Experience Platform サンドボックスが正しく表示されない問題を修正しました。このバージョンの拡張機能を使用する場合、予期されたサンドボックスがリストに表示されない場合は、ユーザーは Adobe Experience Platform 管理者に問い合わせて、アクセス権限が正しく設定されていることを確認する必要があります。

## バージョン2.1.0 - 2020年8月6日

* 重大な変更：`syncIdentity` アクションを削除し、代わりに `sendEvent` アクションにこれらの ID を渡すことをサポートします。拡張機能をアップグレードする前に、この操作を使用する既存のルールを無効にしてください。
* Ally v. 2.1.0 へのアップデート（[リリースノート](https://experienceleague.adobe.com/docs/experience-platform/edge/release-notes.html)）。
* `setConsent` アクションでの IAB 2.0 Consent Standard をサポートします。
* `sendEvent` アクションでのデータセット ID の上書きをサポートします。
* タイプ `IdentityMap` の新しいデータ要素を追加します。これは、現在有効になっている XDM オブジェクトデータ要素と `setConsent` アクションに `identityMap` エントリを入力するために使用できます。
* `setConsent` アクションで ID マップを渡す機能をサポートします。
* XDMオブジェクトデータ要素でのPlatformサンドボックスの選択をサポートします。

## バージョン1.0.0 - 2020年5月27日

* Configuration Service からの環境の選択をサポートします。

## バージョン0.1.2 - 2020年5月5日

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
* `_satellite`を使用したデバッグを有効にすると、Adobe Experience Platform Web SDKでデバッグが有効になります。
* XDM オブジェクトに入力された値（ブール値、数字、および小数）のサポートを追加しました。

## バージョン0.0.10 - 2020年3月17日

* `Consent` の下にオプトインとオプトアウトの概念を組み合わせ、`setConsent` コマンドを追加しました。
* JavaScript／JSON から XDM へのマッピングを可能にする、新しいタイプ `XDM Object` のデータ要素を追加しました。

## バージョン0.0.7 - 2020年2月19日

* Removed idSyncContainerId、datasetId、schemaId、urlDestinationsEnabled、および cookieDestinationsEnabled オプションが削除されました。
* edgeDomain オプション値でのハイフンのサポートを追加しました。
* ID の移行中におこなわれるリクエストは、demdex cookie が設定されていない場合にクロスドメイン識別を改善するために demdex エンドポイントに送信されます。
* ID の移行中におこなわれるリクエストでは常に、ID cookie が設定されることを確認する応答を必要とします。
* 無効なコマンドを実行すると、有効なコマンド名のリストがコンソールに記録されます。
* サードパーティcookieのサポートをタグ拡張に切り替えるためのチェックボックスが追加されました。 これにより、demdex.net への呼び出しが無効になります。

## バージョン0.0.5 - 2019年12月21日

* アクティビティトラッカーの設定をタグ拡張に追加
* イベントコマンドで EventType と EventMergeId を公開。
* タグ拡張にonBeforeEventSend設定を追加
* タグ拡張にedgeBasePath設定を追加

## バージョン0.0.3 - 2019年11月26日

* 「Send Event」アクションの新しい「Merge ID」フィールドと「Type」フィールド。「Merge ID」は XDM スキーマ内の `xdm.eventMergeID` に、「Type」は XDM スキーマ内の `xdm.eventType` にマッピングします。

## バージョン0.0.2 - 2019年11月19日

* 初回リリース
