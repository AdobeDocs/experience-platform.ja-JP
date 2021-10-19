---
audience: user
user-guide-title: 宛先ガイド
user-guide-description: チャネル間のマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告など、様々な用途に使用する既知または不明なデータをアクティブ化します。
description: このドキュメントでは、Adobe エクスペリエンスプラットフォームについて説明する目次が記載されています。
feature: Destinations
source-git-commit: a7c36f1a157b6020fede53e5c1074d966f26cf3d
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 46%

---


# 宛先 {#destinations}

* [Destinations overview](./home.md)
* [宛先のタイプとカテゴリ](./destination-types.md)
* API チュートリアル {#api}
   * [ストリーミングサービス API を使用したストリーミング宛先への接続とデータの有効化](./api/streaming-destinations.md)
   * [電子メールマーケティング宛先に接続し、フローサービス API を使用してデータを有効にします。](./api/email-marketing.md)
* UI ガイド {#ui}
   * [宛先ワークスペース](./ui/destinations-workspace.md)
   * [新しい宛先接続の作成](./ui/connect-destination.md)
   * 宛先への対象ユーザーデータのアクティブ化{#activate}
      * [Activation の概要](./ui/activation-overview.md)
      * [ストリーミングセグメント書き出しの宛先に対するオーディエンスデータのアクティブ化](./ui/activate-segment-streaming-destinations.md)
      * [ストリーミングプロファイルの書き出し先に対する対象ユーザーデータのアクティブ化](./ui/activate-streaming-profile-destinations.md)
      * [対象ユーザーデータをバッチプロファイルの書き出し先にアクティブ化](./ui/activate-batch-profile-destinations.md)
      * [対象ユーザーデータをプロファイルの要求宛先にアクティブ化 (ベータ版)](./ui/activate-profile-request-destinations.md)
   * [宛先情報の表示](./ui/destination-details-page.md)
   * [アップグレード先のアカウントの更新](./ui/update-accounts.md)
   * [アクティベーションフローの編集](./ui/edit-activation.md)
   * [ターゲットの削除](./ui/delete-destinations.md)
   * [データフローの監視](./ui/monitor-dataflows.md)
* 宛先カタログ {#catalog}
   * [宛先カタログの概要](./catalog/overview.md)
   * [ アルファベットHTTP 接続](./catalog/http-destination.md)
   * アドビの宛先 {#adobe}
      * [アドビの宛先の概要](./catalog/adobe/overview.md)
      * [版Marketo 接続の共同で行います。](./catalog/adobe/marketo-engage.md)
      * [プラットフォームセグメント共有の操作](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html?lang=ja)
   * 広告の宛先 {#advertising}
      * [広告の宛先の概要](./catalog/advertising/overview.md)
      * [Adobe Advertising Cloud 拡張機能](./catalog/advertising/adobe-advertising-cloud.md)
      * [Awin Advertiser コンバージョンタグの拡張](./catalog/advertising/awin-conversiontag.md)
      * [Awin Advertiser Mastertag 拡張機能](./catalog/advertising/awin-mastertag.md)
      * [Bing Ads ユニバーサルイベントトラッキング（UET）拡張機能](./catalog/advertising/bing-ads.md)
      * [Branch 拡張機能](./catalog/advertising/branch.md)
      * [DoubleClick Floodlight（ベータ版）拡張機能](./catalog/advertising/doubleclick-floodlight.md)
      * [Facebook ピクセル拡張機能](./catalog/advertising/facebook-pixel.md)
      * [Flashtalking OneTag 拡張機能](./catalog/advertising/flashtalking.md)
      * [Google Ads 接続](./catalog/advertising/google-ads-destination.md)
      * [Google 広告拡張機能](./catalog/advertising/google-ads-extension.md)
      * [Google Ad Manager の接続](./catalog/advertising/google-ad-manager.md)
      * [Google カスタマーが接続に一致](./catalog/advertising/google-customer-match.md)
      * [Google Display &amp; Video 360 の接続](./catalog/advertising/google-dv360.md)
      * [Google gtag 拡張](./catalog/advertising/gtag-advertising.md)
      * [LinkedIn Insight タグ拡張機能](./catalog/advertising/linkedin.md)
      * [Microsoft Bing 接続](./catalog/advertising/bing.md)
      * [Pinterest Conversion Tracking 拡張機能](./catalog/advertising/pinterest-extension.md)
      * [ユーザーリストの pinterest の接続](./catalog/advertising/pinterest.md)
      * [荷馬机の接続](./catalog/advertising/tradedesk.md)
      * [Twitter ユニバーサルウェブサイトタグ拡張](./catalog/advertising/twitter-uwt.md)
      * [Yahoo/Verizon DataX の接続](./catalog/advertising/datax.md)
   * Analytics の宛先 {#analytics}
      * [Analytics の宛先の概要](./catalog/analytics/overview.md)
      * [Adform Website Tracking 拡張機能](./catalog/analytics/adform.md)
      * [Adobe Analytics 拡張機能](./catalog/analytics/adobe-analytics.md)
      * [Adobe Media Analytics for Audio and Video 拡張機能](./catalog/analytics/adobe-video-analytics.md)
      * [Clicktale 拡張機能](./catalog/analytics/clicktale.md)
      * [Contentsquare 拡張機能](./catalog/analytics/contentsquare.md)
      * [Decibel 拡張機能](./catalog/analytics/decibel.md)
      * [Demandbase 拡張機能](./catalog/analytics/demandbase.md)
      * [DialogTech 拡張機能](./catalog/analytics/dialogtech.md)
      * [Google Global Site Tag 拡張機能](./catalog/analytics/gtag-analytics.md)
      * [Google Universal Analytics 拡張機能](./catalog/analytics/google-universal-analytics.md)
      * [JW Player Analytics（ベータ版）拡張機能](./catalog/analytics/jw-player-analytics.md)
      * [Nielsen BSDK 拡張機能](./catalog/analytics/nielsen-bsdk.md)
      * [Nielsen IMA Handler 拡張機能](./catalog/analytics/nielsen-ima.md)
      * [Nielsen VideoJS Player Handler 拡張機能](./catalog/analytics/nielsen-videojs.md)
      * [Parse.ly Analytics 拡張機能](./catalog/analytics/parsely.md)
      * [Quantum Metric 拡張機能](./catalog/analytics/quantum-metric.md)
      * [SessionCam 拡張機能](./catalog/analytics/sessioncam.md)
      * [TMMData 拡張機能](./catalog/analytics/tmmdata.md)
      * [Yext コンバージョントラッキング拡張機能](./catalog/analytics/yext.md)
   * クラウドストレージの宛先 {#cloud-storage}
      * [クラウドストレージの宛先の概要](./catalog/cloud-storage/overview.md)
      * [版Amazon Kinesis 接続](./catalog/cloud-storage/amazon-kinesis.md)
      * [Amazon S3 の接続](./catalog/cloud-storage/amazon-s3.md)
      * [Azure Blob の接続](./catalog/cloud-storage/azure-blob.md)
      * [版Azure Event Hub の接続](./catalog/cloud-storage/azure-event-hubs.md)
      * [SFTP 接続](./catalog/cloud-storage/sftp.md)
      * [IP アドレス許可リスト](./catalog/cloud-storage/ip-address-allow-list.md)
   * データ管理プラットフォームの宛先 {#data-management}
      * [データ管理プラットフォーム (DMP) 宛先の概要](./catalog/data-management/overview.md)
      * [Audience Manager DIL 拡張機能](./catalog/data-management/aam-dil-extension.md)
   * 電子メールの宛先 {#email}
      * [Bizible 拡張機能](./catalog/email/bizible.md)
      * [Marketo 拡張機能](./catalog/email/marketo.md)
      * [Marketo Munchkin 拡張機能](./catalog/email/marketo-munchkin.md)
      * [PebblePost 拡張機能](./catalog/email/pebblepost.md)
   * 電子メールマーケティングの宛先 {#email-marketing}
      * [電子メールマーケティングの宛先の概要](./catalog/email-marketing/overview.md)
      * [Adobe キャンペーンの接続](./catalog/email-marketing/adobe-campaign.md)
      * [Oracle が接続を行います。](./catalog/email-marketing/oracle-eloqua.md)
      * [Oracle の「通信」の接続](./catalog/email-marketing/oracle-responsys.md)
      * [Salesforce マーケティングクラウド接続](./catalog/email-marketing/salesforce-marketing-cloud.md)
   * タグの拡張機能 {#launch-extensions}
      * [タグの拡張の概要](./catalog/launch-extensions/overview.md)
   * Mobile engagement 送信先 {#mobile-engagement}
      * [Mobile engagement 送信先の概要](./catalog/mobile-engagement/overview.md)
      * [Airship 属性の接続](./catalog/mobile-engagement/airship-attributes.md)
      * [Airship タグの接続](./catalog/mobile-engagement/airship-tags.md)
      * [ロウ ze 接続](./catalog/mobile-engagement/braze.md)
   * パーソナライズ機能の宛先 {#personalization}
      * [パーソナライズ機能の宛先の概要](./catalog/personalization/overview.md)
      * [Adobe ターゲット接続 (ベータ版)](./catalog/personalization/adobe-target-connection.md)
      * [Adobe Target 拡張機能](./catalog/personalization/adobe-target.md)
      * [Adobe Target v2 拡張機能](./catalog/personalization/adobe-target-v2.md)
      * [Beemray 拡張機能](./catalog/personalization/beemray.md)
      * [カスタムの個人用設定接続 (ベータ版)](./catalog/personalization/custom-personalization.md)
      * [D&amp;B 訪問者インテリジェンス拡張機能](./catalog/personalization/dnb.md)
      * [Experience Cloud ID サービス拡張機能](./catalog/personalization/adobe-ecid.md)
      * [Gainsight 拡張機能](./catalog/personalization/gainsight.md)
      * [KickFire 拡張機能](./catalog/personalization/kickfire.md)
      * [Marketo Web パーソナライゼーション拡張機能](./catalog/personalization/marketo-web-personalization.md)
   * ソーシャルの宛先{#social}
      * [ソーシャル宛先の概要](./catalog/social/overview.md)
      * [Adobe Livefyre 拡張機能](./catalog/social/adobe-livefyre.md)
      * [Facebook 接続](./catalog/social/facebook.md)
      * [LinkedIn に一致した対象ユーザーの接続](./catalog/social/linkedin.md)
      * [[!DNL Twitter Custom Audiences] 接続](./catalog/social/twitter.md)
   * 調査先 {#survey}
      * [調査先の概要](./catalog/survey/overview.md)
      * [予想拡張出力先](./catalog/survey/foresee.md)
      * [InMoment 拡張機能](./catalog/survey/inmoment.md)
      * [Qualtrics Website Feedback 拡張機能](./catalog/survey/qualtrics.md)
      * [QuestionPro Intercept Surveys 拡張機能](./catalog/survey/web-intercept-surveys.md)
   * 顧客の声の宛先 {#voice}
      * [お客様宛先の声の概要](./catalog/voice/overview.md)
      * [Confirmit Digital Feedback 拡張機能](./catalog/voice/confirmit-digital-feedback.md)
      * [Invoca Tags 拡張機能](./catalog/voice/invoca.md)
      * [Medallia 拡張機能](./catalog/voice/medallia.md)
      * [Talk URL Inbox 拡張機能](./catalog/voice/talkurl.md)
* ターゲット SDK {#destination-sdk}
   * [概要](./destination-sdk/overview.md)
   * [統合の前提条件](./destination-sdk/integration-prerequisites.md)
   * [はじめに](./destination-sdk/getting-started.md)
   * ターゲット SDK 機能 {#functionality}
      * [設定オプション](./destination-sdk/configuration-options.md)
      * [移行先の構成](./destination-sdk/destination-configuration.md)
      * [サーバーとテンプレートの仕様](./destination-sdk/server-and-template-configuration.md)
      * [メッセージのフォーマット](./destination-sdk/message-format.md)
      * [オーディエンスメタデータの管理](./destination-sdk/audience-metadata-management.md)
      * [認証設定](./destination-sdk/credentials-configuration.md)
      * [OAuth 2 認証](./destination-sdk/oauth2-authentication.md)
      * 開発者向けツール {#developer-tools}
         * [メッセージ変換テンプレートの作成とテスト](./destination-sdk/create-template.md)
         * [ターゲットの構成のテスト](./destination-sdk/test-destination.md)
   * API リファレンス {#api-reference}
      * [宛先エンドポイント API 操作](./destination-sdk/destination-configuration-api.md)
      * [移行先サーバーのエンドポイント API 操作](./destination-sdk/destination-server-api.md)
      * [対象ユーザー向けメタデータのエンドポイント API 操作](./destination-sdk/audience-metadata-api.md)
      * [資格情報エンドポイント API 操作](./destination-sdk/credentials-configuration-api.md)
      * [エンドポイント API 操作のパブリッシュ](./destination-sdk/destination-publish-api.md)
      * Developer tools リファレンス {#developer-tools-reference}
         * [サンプルテンプレート API 操作の取得](./destination-sdk/sample-template-api.md)
         * [レンダリングテンプレート API の操作](./destination-sdk/render-template-api.md)
         * [移行先テスト API 操作](./destination-sdk/destination-testing-api.md)
         * [サンプルのプロファイル生成 API 操作](./destination-sdk/sample-profile-generation-api.md)
   * ガイド {#guides}
      * [目的の SDK を使用してストリーミング出力先を設定します。](./destination-sdk/configure-destination-instructions.md)
   * 保存先のドキュメント化 {#document-destination}
      * [Adobe エクスペリエンスプラットフォームでの保存先のドキュメント化](./destination-sdk/docs-framework/documentation-instructions.md)
      * [「接続先のドキュメント」ページを作成するための GitHub web インターフェイスの使用](./destination-sdk/docs-framework/use-github-interface-to-create-documentation.md)
      * [「送信先ドキュメント」ページを作成するには、ローカル環境でテキストエディターを使用してください。](./destination-sdk/docs-framework/work-in-local-environment.md)
      * [ドキュメントセルフサービステンプレート](./destination-sdk/docs-framework/self-service-template.md)
* [よくある質問](./destinations-faq.md)
* [Platform リリースノート](https://docs.adobe.com/content/help/ja-JP/experience-platform/release-notes/latest.html)
