---
title: Adobe Experience Platform Assurance トラブルシューティングガイド
description: このドキュメントでは、Adobe Experience Platform Assurance を使用する際の一般的な問題の解決策の概要を説明します。
exl-id: 8078e6f6-ca18-4939-a417-40ebf5a52e24
source-git-commit: 05a7b73da610a30119b4719ae6b6d85f93cdc2ae
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 1%

---

# Adobe Experience Platform Assurance のトラブルシューティング

Assurance が機能しない場合は、一般的に発生する問題を解決するために、次の問題のトピックの候補を参照してください。

よりスムーズな実装を有効にし、潜在的な問題を見つけるには、「はじめに」セクションの [ デバッグログを有効にする ](https://developer.adobe.com/client-sdks/documentation/getting-started/enable-debug-logging/) に従って SDK ログをオンにします。

SDK API を使用して SDK ログレベルを変更 [`setLogLevel`](https://developer.adobe.com/client-sdks/documentation/mobile-core/api-reference/#setloglevel) きます。

## オンデバイス、アプリの問題

### QR コードがアプリを開かない

* リンクに直接アクセスします。 **セッションの詳細** からリンクをコピーします。 デバイスのブラウザーアドレスバーに貼り付けて開きます。 開かない場合は、[ アプリがリンクを開かない ](#app-does-not-open-link) の節を参照してください。
* 別の QR リーダーを使用してください。 iOS 11 以降の場合は、フォトアプリを使用して QR コードを読み取ります。

### アプリがリンクを開かない

* アプリでディープリンク実装が正しく設定されていることを確認します。
   * **Android:** 個のディープリンク（アプリのリンク）
      * [ アプリコンテキストへのディープリンクの作成 ](https://developer.android.com/training/app-links/deep-linking)
   * **iOS:** カスタム URL スキームまたはユニバーサル リンク
      * [ アプリのカスタム URL スキームの定義 ](https://developer.apple.com/documentation/uikit/inter-process_communication/allowing_apps_and_websites_to_link_to_your_content/defining_a_custom_url_scheme_for_your_app)
      * [ アプリ内のユニバーサルリンクのサポート ](https://developer.apple.com/documentation/uikit/inter-process_communication/allowing_apps_and_websites_to_link_to_your_content/supporting_universal_links_in_your_app)

>[!INFO]
>
>Androidの場合、`startSession` API を明示的に呼び出す必要はありません。 iOSの場合は、[Adobe Experience Platform Assurance](https://developer.adobe.com/client-sdks/documentation/platform-assurance-sdk/#register-aepassurance-with-mobile-core) の説明に従って API を使用します。

### 認証オーバーレイが表示されるが、アプリの接続に失敗する

* デバイスの Web ブラウザを使用して、デバイスのインターネット接続を確認します。
* アプリが Assurance サービスに正常に接続したことがない場合は、Assurance が正しく設定されていることを確認します。 [Adobe Experience Platform Assurance](./tutorials/implement-assurance.md) SDK ライブラリのインストール手順を参照してください。
* セッションがリンクと一致し、期待されるセッションに対して正しく入力されていることを確認します。 [ ログメッセージ「OrgID 情報を利用できません」 ](https://developer.adobe.com/client-sdks/documentation/platform-assurance-sdk/common-issues/#orgid-information-is-not-available) を参照してください（これは一般的ではなく、複数の ORG インスタンスにアクセスできる場合にのみ関係します）。

### Adobe Analyticsのデバッグ

**Post処理ステータス – デバッグフラグなし**

Analytics イベントビューで、Post処理ステータスが「デバッグフラグなし」のイベントが失敗した場合、現在のAdobe Analyticsまたは Assurance SDK のバージョンが Analytics のデバッグ機能をサポートしていない可能性があります。
この問題を解決するには、Adobe Analyticsおよび Assurance SDK の拡張機能を最新バージョンにアップグレードしてください。

| 最小バージョン要件 | iOS | Android |
| --------------------------- | --- | ------- |
| Adobe Analytics | > 2.4.0 | > 1.2.6 |
| Assurance | > 1.0.0 | > 1.0.0 |

### React Native MobileCore と AEPAssurance の互換性

| AEP Assurance バージョン | Mobile Core バージョン | インストール手順 |
| --------------------- | ------------------- | ------------------- |
| react-native-aepassurance v2.x.x | [react-native-acpcore](https://www.npmjs.com/package/@adobe/react-native-acpcore) | `npm install @adobe/react-native-aepassurance@^2.0.0` <br/>`npm install @adobe/react-native-acpcore` |
| react-native-aepassurance v3.x.x | [react-native-aepcore](https://www.npmjs.com/package/@adobe/react-native-aepcore) | `npm install @adobe/react-native-aepassurance@^3.0.0` <br/>`npm install @adobe/react-native-aepcore` |

Assurance で `react-native-acpcore` を使用している場合、次のいずれかのエラーメッセージでReact Native アプリケーションのビルドに失敗する場合があります。

```
RCTAEPAssurance:  Fatal error: Module 'AEPAssurance' not found
```

または

```
AppDelegate: AEPAssurance.h file not found
```

**ソリューション**

その場合は、次の npm コマンドを使用して `react-native-aepassurance` をダウングレードしてください。

```shell
npm install @adobe/react-native-aepassurance@^2.0.0
```

このエラーは、`react-native-acpcore` の拡張機能が `react-native-aepassurance` バージョン 2.x.x 以下と **のみ** 互換性があるために発生します。
