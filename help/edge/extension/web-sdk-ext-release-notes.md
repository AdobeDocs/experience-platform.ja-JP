---
title: Adobe Experience Platform Web SDK 拡張機能リリースノート
description: Adobe Experience Platform Web SDK タグ拡張機能
exl-id: 91de8c91-023a-45b6-9f67-ac75ee471e50
source-git-commit: ccd02ea014d514b56a8e1bd540bb2c2c4bb2eb1b
workflow-type: tm+mt
source-wordcount: '1654'
ht-degree: 93%

---


# Adobe Experience Platform Web SDK 拡張機能リリースノート

このドキュメントでは、Adobe Experience Platform Web SDK タグ拡張機能のリリースノートについて説明します。SDK 自体の最新のリリースノートについては、[Platform Web SDK リリースノート](https://experienceleague.adobe.com/docs/experience-platform/edge/release-notes.html?lang=ja)を参照してください。

## バージョン2.17.0 - 2023 年 4 月 25 日

**新機能**

* データストリーム設定の上書きのサポートが追加されました。
* 廃止の通知を `datasetId` オプションを `sendEvent` コマンドを使用します。

**修正点および改善点**

* Safari でスクロールすると datastream セレクターが閉じる問題を修正しました。

## バージョン2.16.1 - 2023 年 4 月 14 日

* XDM オブジェクトと変数データ要素で、デフォルト以外のサンドボックスからスキーマを選択できなかった問題を修正しました。

## バージョン2.16.0 - 2023 年 3 月 30 日

**新機能**

* （ベータ版）追加済み **[!UICONTROL 変数を更新]** アクションと **[!UICONTROL 変数]** データ要素。
* 次の設定を追加しました： [`onBeforeLinkClickSend`](../fundamentals/configuring-the-sdk.md#onBeforeLinkClickSend) コールバック関数。

**修正点および改善点**

* アンカータグ内の要素をクリックすると、 **[!UICONTROL ID でリダイレクト]** アクションが使用されました。
* スキーマが 1 つだけ存在する場合に XDM オブジェクトのデータ要素が機能しない問題を修正しました。
* Adobe Experience Platform Web SDK のバージョン 2.15.0 が含まれます。


## バージョン 2.15.1 - 2023年1月26日（PT）

* データストリームにアクセスできないユーザーが拡張機能の設定を編集できない問題を修正しました。
* `sendEvent` アクションにサーフェスのサポートを追加しました。

Adobe Experience Platform Web SDK のバージョン 2.14.0 が含まれます。


## バージョン 2.14.1 - 2022年10月13日（PT）

* Web SDK が Experience Cloud ID サービスからの ID を受け入れない問題を修正しました。

Adobe Experience Platform Web SDK ライブラリのバージョン 2.13.1 が含まれます。

## バージョン 2.14.0 - 2022年9月28日（PT）

* ページごとの完全移行を有効にする新しい `targetMigrationEnabled` 設定を追加しました。
* ハイブリッドサーバーとクライアントの実装を有効にする適用応答アクションを追加しました。
* 高エントロピーの user-agent クライアントヒントコンテキストオプションを追加しました。

Adobe Experience Platform Web SDK ライブラリのバージョン 2.13.0 が含まれます。

## バージョン 2.13.0 - 2022年6月29日（PT）

* eVar などの XDM オブジェクトデータ要素における数値プロパティの並べ替え順序を修正しました。

Adobe Experience Platform Web SDK ライブラリのバージョン 2.12.0 が含まれます。

## バージョン 2.12.0 - 2022年6月13日（PT）

* 拡張機能の設定で定義されたサンドボックスに基づいて名前空間オプションを入力するように、`identityMap` データ要素を更新しました。
* クロスドメイン ID 共有を可能にする **[!UICONTROL ID を使用したリダイレクト]**&#x200B;アクションを追加しました。
* `sendEvent` アクションへのドキュメントリンクを追加しました。
* React Spectrum UI ライブラリをアップグレードしました。
* 複数のユーザーインターフェイスの機能強化。

Adobe Experience Platform Web SDK ライブラリのバージョン 2.11.0 が含まれます。

## バージョン 2.11.2 - 2022年5月3日（PT）

Adobe Experience Platform Web SDK ライブラリのバージョン 2.10.1 が含まれます。

## バージョン 2.11.1 - 2022年4月22日（PT）

* バージョン 2.11.0 からの configure コマンドエラーを修正しました。

Adobe Experience Platform Web SDK ライブラリのバージョン 2.10.0 が含まれます。

## バージョン 2.11.0 - 2022年4月22日（PT）

* タグ UI のパフォーマンスを改善しました。
* サンドボックスセレクターをデータストリーム拡張機能の設定に追加します。

Adobe Experience Platform Web SDK ライブラリのバージョン 2.10.0 が含まれます。

## バージョン 2.10.0 - 2022年3月10日（PT）

* 設定ページでコピーできる事前非表示スニペットを更新して、更新された Adobe Target VEC エディターで動作するようにします。

Adobe Experience Platform Web SDK ライブラリのバージョン 2.9.0 が含まれます。

## バージョン 2.9.0 - 2022年1月19日（PT）

Adobe Experience Platform Web SDK ライブラリのバージョン 2.8.0 が含まれます。

## バージョン 2.8.0 - 2021年10月26日（PT）

Adobe Experience Platform Web SDK ライブラリのバージョン 2.7.0 が含まれます。

* Experience Edge からの追加情報は、`inferences` や `destinations` など、イベント完了を送信イベントで利用できます。これらの機能は現在、ベータ版の一部として公開されているので、これらのプロパティの形式は変更される可能性があります。詳しくは、[トラッキングイベント](../fundamentals/tracking-events.md)を参照してください。

## バージョン 2.7.3 - 2021年9月7日（PT）

Adobe Experience Platform Web SDK ライブラリのバージョン 2.6.4 が含まれます。

* `container.buildInfo.environment.` に対する非推奨の警告はなくなりました。

## バージョン 2.7.0 - 2021年8月16日（PT）

Adobe Experience Platform Web SDK ライブラリのバージョン 2.6.3 が含まれます。

* ID マップデータ要素タイプを使用する場合、ID が入力された文字列ではない値に解決される識別子は、ID マップから自動的に削除されるようになりました。
* XDM オブジェクトデータ要素タイプを使用してデータ要素を保存しようとして、スキーマが選択されていない場合に発生するエラーを修正しました。
* ユーザーインターフェイスのテキスト編集を改善しました。

## バージョン 2.6.2 - 2021年8月4日（PT）

Adobe Experience Platform Web SDK ライブラリのバージョン 2.6.4 が含まれます。

## バージョン 2.6.1 - 2021年7月29日（PT）

Adobe Experience Platform Web SDK ライブラリのバージョン 2.6.1 が含まれます。

## バージョン 2.6.0 - 2021年7月27日（PT）

Adobe Experience Platform Web SDK ライブラリのバージョン 2.6.0 が含まれます。

* 「エッジ設定」という用語を使用するラベル、説明およびエラーメッセージを、最新の Adobe Experience Platform の用語に合わせて「データストリーム」という用語を使用するように変更しました。
* 拡張機能の設定ビューで、多数のデータストリームおよびデータストリーム環境を処理するためのサポートを追加しました。
* XDM オブジェクトデータ要素ビューで、多数のスキーマを処理するためのサポートを追加しました。
* イベント完了を送信イベントタイプを追加しました。これを使用すると、イベントがサーバーに送信され、応答が受信された後にルールを実行できます。近日中に、その他のドキュメントも利用できるようになります。
* 決定を受信済みイベントタイプは非推奨（廃止予定）となりました。代わりに、イベント完了を送信イベントタイプを使用してください。
* ユーザーインターフェイスとエラー処理を全般的に改善しました。

## バージョン 2.5.0 - 2021年6月1日（PT）

Adobe Experience Platform Web SDK ライブラリのバージョン 2.5.0 が含まれます。

* イベントを送信アクションに `data` フィールドを追加しました。今後のドキュメントでは、特定のシナリオで使用する方法について説明します。
* XDM オブジェクトデータ要素ビューで、ユーザーが Adobe Experience Platform サンドボックスにアクセスできても、組織のデフォルトとして設定されたサンドボックスにはアクセスできない場合にエラーがスローされる問題を修正しました。
* XDM オブジェクトデータ要素ビューで、親オブジェクトに値が含まれていない場合でも、必須のスキーマフィールドが無効と見なされる問題を修正しました。

## バージョン 2.4.0 - 2021年3月9日（PT）

Adobe Experience Platform Web SDK ライブラリのバージョン 2.4.0 が含まれます。

* イベントを送信アクション UI に「[ドキュメントのアンロード中](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/tracking-events.html?lang=ja#using-the-sendbeacon-api)」チェックボックスを追加しました。
* [デフォルトの同意を設定](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html?lang=ja#default-consent)する際に、同意が得られるまですべてのイベントをドロップする `out` オプションのサポートを追加しました（既存の `pending` オプションはイベントをキューに入れ、同意が得られると送信します）。
* デフォルトの同意フィールドにツールヒントを追加しました。
* [アドビの同意 2.0 標準](https://experienceleague.adobe.com/docs/experience-platform/edge/consent/supporting-consent.html?lang=ja#communicating-consent-preferences-via-the-adobe-standard)のサポートを追加しました。
* ユーザーのアクセストークンが無効であるか、不適切にプロビジョニングされている場合に、XDM オブジェクトデータ要素 UI に表示されるエラーを改善しました。
* XDM オブジェクトデータ要素を表示する際にブラウザーの開発者コンソールに表示されるクロスオリジンエラー（拡張機能の操作には影響しません）を修正しました。

## バージョン 2.3.0 - 2020年11月4日（PT）

Adobe Experience Platform Web SDK ライブラリのバージョン 2.3.0 が含まれます。

* デフォルトの同意を設定する際のデータ要素の使用に関するサポートを追加しました。
* XDM オブジェクトデータ要素タイプで XDM スキーマを検索する機能を追加しました。
* XDM データオブジェクトに対する以降の変更が要求に反映されないように、「イベントの送信」アクションタイプ内に XDM データのクローンを追加しました。

## バージョン 2.2.0 - 2020年10月1日（PT）

* お客様がサンドボックススキーマから XDM オブジェクトを作成しようとすると、認証の問題が発生していました。Platform を呼び出す API が環境を認識するようになったので、編集にアクセスできるスキーマのみが表示されます。
* `identityMap` データ要素を使用する場合、名前空間はドロップダウンで事前設定されるので、手動で入力する必要がありません。
* `xdmObject` データ要素の UI が改良されました。新しい UI では、オブジェクトに各項目を入力しなくても、入力されたフィールドを確認できます。

## バージョン 2.1.1 - 2020年8月26日（PT）

* XDM オブジェクトビューの Adobe Experience Platform サンドボックスが正しく表示されない問題を修正しました。このバージョンの拡張機能を使用する場合、予期されたサンドボックスがリストに表示されない場合は、ユーザーは Adobe Experience Platform 管理者に問い合わせて、アクセス権限が正しく設定されていることを確認する必要があります。

## バージョン 2.1.0 - 2020年8月5日（PT）

* 重大な変更：`syncIdentity` アクションを削除し、代わりに `sendEvent` アクションにこれらの ID を渡すことをサポートします。拡張機能をアップグレードする前に、この操作を使用する既存のルールを無効にしてください。
* Ally v. 2.1.0 へのアップデート（[リリースノート](https://experienceleague.adobe.com/docs/experience-platform/edge/release-notes.html?lang=ja)）。
* `setConsent` アクションでの IAB 2.0 Consent Standard をサポートします。
* `sendEvent` アクションでのデータセット ID の上書きをサポートします。
* タイプ `IdentityMap` の新しいデータ要素を追加します。これは、現在有効になっている XDM オブジェクトデータ要素と `setConsent` アクションに `identityMap` エントリを入力するために使用できます。
* `setConsent` アクションで ID マップを渡す機能をサポートします。
* XDM オブジェクトデータ要素での Platform サンドボックスの選択をサポートします。

## バージョン 1.0.0 - 2020年5月26日（PT）

* Configuration Service からの環境の選択をサポートします。

## バージョン 0.1.2 - 2020年5月4日（PT）

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
* `_satellite` を使用したデバッグを有効にすると、Adobe Experience Platform Web SDK 機能でデバッグが有効になります。
* XDM オブジェクトに入力された値（ブール値、数字、および小数）のサポートを追加しました。

## バージョン 0.0.10 - 2020年3月16日（PT）

* `Consent` の下にオプトインとオプトアウトの概念を組み合わせ、`setConsent` コマンドを追加しました。
* JavaScript／JSON から XDM へのマッピングを可能にする、新しいタイプ `XDM Object` のデータ要素を追加しました。

## バージョン 0.0.7 - 2020年2月18日（PT）

* Removed idSyncContainerId、datasetId、schemaId、urlDestinationsEnabled、および cookieDestinationsEnabled オプションが削除されました。
* edgeDomain オプション値でのハイフンのサポートを追加しました。
* ID の移行中におこなわれるリクエストは、demdex cookie が設定されていない場合にクロスドメイン識別を改善するために demdex エンドポイントに送信されます。
* ID の移行中におこなわれるリクエストでは常に、ID cookie が設定されることを確認する応答を必要とします。
* 無効なコマンドを実行すると、有効なコマンド名のリストがコンソールに記録されます。
* サードパーティ cookie のサポートをタグ拡張に切り替えるためのチェックボックスを追加しました。これにより、demdex.net への呼び出しが無効になります。

## バージョン 0.0.5 - 2019年12月20日（PT）

* タグ拡張機能にアクティビティトラッカー設定を追加しました
* イベントコマンドで EventType と EventMergeId を公開。
* タグ拡張機能に onBeforeEventSend 設定を追加しました
* タグ拡張機能に edgeBasePath 設定を追加しました

## バージョン 0.0.3 - 2019年11月25日（PT）

* 「Send Event」アクションの新しい「Merge ID」フィールドと「Type」フィールド。「Merge ID」は XDM スキーマ内の `xdm.eventMergeID` に、「Type」は XDM スキーマ内の `xdm.eventType` にマッピングします。

## バージョン 0.0.2 - 2019年11月18日（PT）

* 初回リリース
