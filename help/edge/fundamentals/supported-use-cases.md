---
title: Adobe Experience Platform Web SDKでサポートされる使用例
description: Adobe Experience Platform Web SDKでサポートされる使用例を説明します。
keywords: web sdk；使用例
exl-id: e0643c2c-ceb3-4ea2-aafa-1e18e0c66453
source-git-commit: ed092b85d74eaa0fdc29f3a8d28f84fe81ccca17
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 20%

---

# サポートされるユースケース

このページでは、Web SDKでサポートされる使用例と追加情報へのリンクを示します。

## 全般

| 使用例 | 詳細情報 |
| --- | --- |
| 合理化された1つのSDK |  |
| グローバルデータ収集ネットワーク |  |
| コースの詳細な同意 |  |
| 様々な標準に基づいて顧客の同意を得る | <ul><li>[Adobeの同意2.0のサポート](../../landing/governance-privacy-security/consent/adobe/overview.md)</li><li>[IAB TCF 2.0のサポート](../../landing/governance-privacy-security/consent/iab/overview.md)</li><li>[Edgeネットワークに同意シグナルを送信するためのSDKの統合](../../landing/governance-privacy-security/consent/sdk.md)</li></ul> |
| ECIDのサポート | ECIDの取得について詳しくは、[こちら](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html?lang=en#first-party-identity)および[こちら](https://experienceleague.adobe.com/docs/experience-platform/edge/extension/accessing-the-ecid.html?lang=en#extension)を参照してください。 |
| 複数エンティティの収集 |  |
| デバイスグラフのサポート（公開/非公開） | [ドキュメント](https://experienceleague.adobe.com/docs/analytics/components/cda/device-graph.html?lang=en) |
| ページ上の複数の組織へのデータ送信 | [ドキュメント](./interacting-with-multiple-properties.md) |
| 詳細なエラーレポートとログ |  |
| クライアント側およびサーバー側の要求をトレースする |  |
| タグ拡張 | [Web SDK拡張機能のドキュメント](../../tags/extensions/web/sdk/overview.md) |
| 使用可能なデバッグツール | [Debugger拡](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html?lang=en) 張機能と [Griffon](https://aep-sdks.gitbook.io/docs/beta/project-griffon) |

{style=&quot;table-layout:auto&quot;}

## Adobe Experience Platform

| 使用例 | 詳細情報 |
| --- | --- |
| エクスペリエンスイベントの送信 |  |
| Offer Decisioning | [ドキュメント](../personalization/offer-decisioning/offer-decisioning-overview.md) |
| プロファイルに対してデータセットが有効になっている場合、リアルタイム顧客データプロファイルにデータを送信する機能 |  |
| データをCustomer Journey Analyticsにリアルタイムで送信 |  |
| プロファイルへの書き込み同意 | [ドキュメント](../../landing/governance-privacy-security/consent/sdk.md) |
| サーバーサイドでリアルタイムにサードパーティにデータを転送する | [ドキュメント](../../tags/ui/event-forwarding/overview.md) |
| ID名前空間のサポート |  |

{style=&quot;table-layout:auto&quot;}

## Adobe Analytics

| 使用例 | 詳細情報 |
| --- | --- |
| Analytics for Target（A4T） |  |
| Analytics for Target(A4T)の遅延なし |  |
| マルチスイートタギング |  |
| ボットフィルタリング |  |
| prop、eVar、イベント |  |
| ListVarによるAdobe Analyticsのサポート |  |
| OSとブラウザーのバージョン |  |
| 標準の変数 | [自動的にマッピングされた変数](../data-collection/adobe-analytics/automatically-mapped-vars.md) |
| VISTAルール/処理ルール |  |
| 訪問者属性のサポート |  |
| 離脱リンクのサポート |  |
| カスタムリンク/ダウンロードリンク |  |
| 状態とアクションの追跡 |  |
| 標準イベントのイベントのシリアル化 |  |
| products 変数 | [ドキュメント](../data-collection/collect-commerce-data.md#actions-related-to-products) |

{style=&quot;table-layout:auto&quot;}

## Adobe Target

| 使用例 | 詳細情報 |
| --- | --- |
| すべてのアクティビティのタイプ |  |
| ネイティブおよびSPA Visual Experience Composerのサポート | [ドキュメント](../personalization/adobe-target/spa-implementation.md) |
| フォームベースのコンポーザー |  |
| グローバルmboxのサポート | [ドキュメント](../personalization/rendering-personalization-content.md#automatically-rendering-content) |
| カスタム mbox | [ドキュメント](../personalization/rendering-personalization-content.md#manually-rendering-content) |
| Analytics for Target（A4T） |  |
| 環境のサポート |  |
| Workspaceのサポート |  |
| Adobe TargetのQAリンク |  |
| Adobe Targetの地域/デバイスに基づくターゲット設定 |  |
| 訪問者属性のサポート |  |
| プロファイルスクリプト |  |
| XDMがmboxパラメーターになる |  |
| A4Tレポートでサポートされるリダイレクトオファー | [ドキュメント](https://experienceleague.adobe.com/docs/target/using/experiences/offers/offer-redirect.html?lang=en) |
| Targetプロファイルの更新 | [ドキュメント](../personalization/adobe-target/target-overview.md#single-profile-update) |
| 推奨事項 |  |
| mboxサードパーティID |  |
| レスポンストークン | [ドキュメント](../personalization/adobe-target/accessing-response-tokens.md) |

{style=&quot;table-layout:auto&quot;}

## Adobe Audience Manager

| 使用例 | 詳細情報 |
| --- | --- |
| Audience Analytics |  |
| Adobe Analyticsへのセグメント共有 |  |
| 訪問者属性のサポート |  |
| パートナーの同期 |  |
| URLの宛先 |  |
| Cookieの宛先 |  |
| 環境のサポート |  |
| Adobe Experience Platform名前空間とAudience Managerデータソースの同期 |  |
| 認証済みIDまたは既知のID |  |

{style=&quot;table-layout:auto&quot;}
