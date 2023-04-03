---
title: Adobe Experience Platform Assurance トラブルシューティングガイド
description: このドキュメントでは、Adobe Experience Platform Assurance を使用する際の一般的な問題への解決策について説明します。
source-git-commit: 07dc01c11c79ac2dad05d89309cabb5715c0b63c
workflow-type: tm+mt
source-wordcount: '526'
ht-degree: 3%

---


# Adobe Experience Platform Assurance のトラブルシューティング

アシュランスの機能で問題が発生した場合は、次の問題トピックの提案を参照して、よく発生する問題を解決してください。

よりスムーズな実装を有効にし、潜在的な問題を検出するには、 [デバッグログを有効にする](https://developer.adobe.com/client-sdks/documentation/getting-started/enable-debug-logging/) （はじめにの節）を参照してください。

SDK ログレベルは、 [`setLogLevel`](https://developer.adobe.com/client-sdks/documentation/mobile-core/api-reference/#setloglevel) API

## デバイス上、アプリの問題

### QR コードがアプリを開いていません

* リンクに直接アクセスします。 リンクのコピー元 **セッションの詳細**. デバイスのブラウザーのアドレスバーに貼り付けて開きます。 開かない場合は、の節を参照してください。 [アプリはリンクを開けません](#app-does-not-open-link).
* 別の QR リーダーを使用します。 iOS 11 以降の場合は、フォトアプリを使用して QR コードを読み取ります。

### アプリがリンクを開けません

* アプリでディープリンクの実装が正しく設定されていることを確認します。
   * **Android:** ディープリンク（アプリリンク）
      * [アプリコンテキストへのディープリンクの作成](https://developer.android.com/training/app-links/deep-linking)
   * **iOS:** カスタム URL スキームまたはユニバーサルリンク
      * [アプリのカスタム URL スキームの定義](https://developer.apple.com/documentation/uikit/inter-process_communication/allowing_apps_and_websites_to_link_to_your_content/defining_a_custom_url_scheme_for_your_app)
      * [アプリでのユニバーサルリンクのサポート](https://developer.apple.com/documentation/uikit/inter-process_communication/allowing_apps_and_websites_to_link_to_your_content/supporting_universal_links_in_your_app)

>[!INFO]
>
>Android の場合、 `startSession` API を明示的に呼び出す必要はありません。 iOSの場合は、 [Adobe Experience Platform Assurance](https://developer.adobe.com/client-sdks/documentation/platform-assurance-sdk/#register-aepassurance-with-mobile-core).

### 認証オーバーレイが表示されるが、アプリが接続に失敗する

* デバイスの Web ブラウザーを使用して、デバイスのインターネット接続を確保します。
* アプリが Assurance サービスに正常に接続していない場合は、Assurance が正しく設定されていることを確認します。 詳しくは、 [Adobe Experience Platform Assurance](./tutorials/implement-assurance.md) SDK ライブラリ。
* セッションがリンクと一致し、期待されるセッションに対して正しく入力されていることを確認します。 詳しくは、 [ログメッセージ「OrgID information is not available」](https://developer.adobe.com/client-sdks/documentation/platform-assurance-sdk/common-issues/#orgid-information-is-not-available) （これはまれで、複数の ORG インスタンスにアクセスできる場合にのみ関連します）。

### Adobe Analytics Debugging

**後処理ステータス — デバッグフラグなし**

Analytics イベント表示で、イベントが後処理ステータス「デバッグフラグなし」で失敗した場合、現在のAdobe Analyticsまたは Assurance SDK バージョンで Analytics デバッグ機能がサポートされていない可能性があります。
この問題を解決するには、Adobe Analyticsおよび Assurance SDK の拡張機能を最新バージョンにアップグレードしてください。

| 最小バージョン要件 | iOS | Android |
| --------------------------- | --- | ------- |
| Adobe Analytics | > 2.4.0 | > 1.2.6 |
| Assurance | > 1.0.0 | > 1.0.0 |

### React Native MobileCore と AEPAssurance の互換性

| AEP アシュランスバージョン | Mobile Core バージョン | インストール手順 |
| --------------------- | ------------------- | ------------------- |
| react-native-aepassurance v2.x.x | [react-native-acpcore](https://www.npmjs.com/package/@adobe/react-native-acpcore) | `npm install @adobe/react-native-aepassurance@^2.0.0` <br/>`npm install @adobe/react-native-acpcore` |
| react-native-aepassurance v3.x.x | [react-native-aepcore](https://www.npmjs.com/package/@adobe/react-native-aepcore) | `npm install @adobe/react-native-aepassurance@^3.0.0` <br/>`npm install @adobe/react-native-aepcore` |

次を使用する場合： `react-native-acpcore` アシュランスを使用すると、React Native アプリケーションは、次のいずれかのエラーメッセージでビルドに失敗する場合があります。

```
RCTAEPAssurance:  Fatal error: Module 'AEPAssurance' not found
```

or

```
AppDelegate: AEPAssurance.h file not found
```

**ソリューション**

その場合は、 `react-native-aepassurance` 次の npm コマンドを使用します。

```shell
npm install @adobe/react-native-aepassurance@^2.0.0
```

このエラーは、 `react-native-acpcore` 拡張子は **のみ** ～と互換性がある `react-native-aepassurance` バージョン 2.x.x 以前。
