---
audience: user
user-guide-title: 宛先ガイド
user-guide-description: チャネル間のマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告など、様々な用途に使用する既知または不明なデータをアクティブ化します。
description: このドキュメントでは、Adobe Experience Platformの宛先の目次を示します
feature: Destinations
source-git-commit: 628e7a993a3566322e0249a5a9864cf6b3fe4493
workflow-type: tm+mt
source-wordcount: '695'
ht-degree: 45%

---


# 宛先 {#destinations}

* [宛先の概要](./home.md)
* [宛先のタイプとカテゴリ](./destination-types.md)
* API チュートリアル {#api}
   * [フローサービス API を使用して、ストリーミング宛先に接続し、データをアクティブ化する](./api/streaming-destinations.md)
   * [フローサービス API を使用して、電子メールマーケティングの宛先に接続し、データをアクティブ化する](./api/email-marketing.md)
   * [（ベータ版）アドホックアクティベーション API を介して、バッチ配信先に対するオーディエンスセグメントをアクティブ化します](./api/ad-hoc-activation-api.md)
   * [宛先データフローを更新](./api/update-destination-dataflows.md)
   * [宛先アカウントの削除](./api/delete-destination-account.md)
   * [宛先データフローを削除](./api/delete-destination-dataflow.md)
* UI ガイド {#ui}
   * [宛先ワークスペース](./ui/destinations-workspace.md)
   * [新しい宛先接続を作成](./ui/connect-destination.md)
   * 宛先へのオーディエンスデータのアクティブ化{#activate}
      * [Activation の概要](./ui/activation-overview.md)
      * [ストリーミングセグメント書き出しの宛先に対するオーディエンスデータのアクティブ化](./ui/activate-segment-streaming-destinations.md)
      * [ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化](./ui/activate-streaming-profile-destinations.md)
      * [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](./ui/activate-batch-profile-destinations.md)
      * [プロファイルリクエストの宛先に対するオーディエンスデータのアクティブ化（ベータ版）](./ui/activate-profile-request-destinations.md)
      * [同じページと次のページのパーソナライゼーション用にパーソナライゼーションの宛先を設定](./ui/configure-personalization-destinations.md)
   * [宛先の詳細を表示](./ui/destination-details-page.md)
   * [宛先アカウントの更新](./ui/update-accounts.md)
   * [アクティベーションフローを編集](./ui/edit-activation.md)
   * [宛先の削除](./ui/delete-destinations.md)
   * [データフローの監視](./ui/monitor-dataflows.md)
* 宛先カタログ {#catalog}
   * [宛先カタログの概要](./catalog/overview.md)
   * アドビの宛先 {#adobe}
      * [アドビの宛先の概要](./catalog/adobe/overview.md)
      * [Marketo Engage接続](./catalog/adobe/marketo-engage.md)
      * [Experience Platformセグメントの共有](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html?lang=ja)
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
      * [Google Ad Manager 接続](./catalog/advertising/google-ad-manager.md)
      * [Google Customer Match の接続](./catalog/advertising/google-customer-match.md)
      * [Google Display と Video 360 の接続](./catalog/advertising/google-dv360.md)
      * [Google gtag 拡張](./catalog/advertising/gtag-advertising.md)
      * [LinkedIn Insight タグ拡張機能](./catalog/advertising/linkedin.md)
      * [Microsoft Bing 接続](./catalog/advertising/bing.md)
      * [Pinterest Conversion Tracking 拡張機能](./catalog/advertising/pinterest-extension.md)
      * [Pinterest Customer List の接続](./catalog/advertising/pinterest.md)
      * [トレードデスクの連携](./catalog/advertising/tradedesk.md)
      * [Twitter ユニバーサルウェブサイトタグ拡張](./catalog/advertising/twitter-uwt.md)
      * [Yahoo/Verizon DataX 接続](./catalog/advertising/datax.md)
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
      * [（ベータ版）Amazon Kinesis接続](./catalog/cloud-storage/amazon-kinesis.md)
      * [Amazon S3 接続](./catalog/cloud-storage/amazon-s3.md)
      * [Azure BLOB 接続](./catalog/cloud-storage/azure-blob.md)
      * [（ベータ版）Azure Event Hubs 接続](./catalog/cloud-storage/azure-event-hubs.md)
      * [SFTP 接続](./catalog/cloud-storage/sftp.md)
      * [IP アドレスの許可リスト](./catalog/cloud-storage/ip-address-allow-list.md)
   * データ管理プラットフォームの宛先 {#data-management}
      * [データ管理プラットフォーム (DMP) の宛先の概要](./catalog/data-management/overview.md)
      * [Audience Manager DIL 拡張機能](./catalog/data-management/aam-dil-extension.md)
   * 電子メールの宛先 {#email}
      * [Bizible 拡張機能](./catalog/email/bizible.md)
      * [Marketo 拡張機能](./catalog/email/marketo.md)
      * [Marketo Munchkin 拡張機能](./catalog/email/marketo-munchkin.md)
      * [PebblePost 拡張機能](./catalog/email/pebblepost.md)
   * 電子メールマーケティングの宛先 {#email-marketing}
      * [電子メールマーケティングの宛先の概要](./catalog/email-marketing/overview.md)
      * [Adobe Campaign接続](./catalog/email-marketing/adobe-campaign.md)
      * [OracleEloqua 接続](./catalog/email-marketing/oracle-eloqua.md)
      * [OracleResponsys 接続](./catalog/email-marketing/oracle-responsys.md)
      * [SalesforceMarketing Cloud接続](./catalog/email-marketing/salesforce-marketing-cloud.md)
   * タグ拡張 {#launch-extensions}
      * [タグ拡張機能の概要](./catalog/launch-extensions/overview.md)
   * モバイルエンゲージメントの宛先 {#mobile-engagement}
      * [モバイルエンゲージメントの宛先の概要](./catalog/mobile-engagement/overview.md)
      * [航空船属性の接続](./catalog/mobile-engagement/airship-attributes.md)
      * [飛行船タグの接続](./catalog/mobile-engagement/airship-tags.md)
      * [接続をブレーズ](./catalog/mobile-engagement/braze.md)
   * パーソナライズ機能の宛先 {#personalization}
      * [パーソナライズ機能の宛先の概要](./catalog/personalization/overview.md)
      * [Adobe Target接続](./catalog/personalization/adobe-target-connection.md)
      * [Adobe Target 拡張機能](./catalog/personalization/adobe-target.md)
      * [Adobe Target v2 拡張機能](./catalog/personalization/adobe-target-v2.md)
      * [Beemray 拡張機能](./catalog/personalization/beemray.md)
      * [カスタムパーソナライゼーション接続](./catalog/personalization/custom-personalization.md)
      * [D&amp;B 訪問者インテリジェンス拡張機能](./catalog/personalization/dnb.md)
      * [Experience Cloud ID サービス拡張機能](./catalog/personalization/adobe-ecid.md)
      * [Gainsight 拡張機能](./catalog/personalization/gainsight.md)
      * [KickFire 拡張機能](./catalog/personalization/kickfire.md)
      * [Marketo Web パーソナライゼーション拡張機能](./catalog/personalization/marketo-web-personalization.md)
   * ソーシャルの宛先{#social}
      * [ソーシャルの宛先の概要](./catalog/social/overview.md)
      * [Adobe Livefyre 拡張機能](./catalog/social/adobe-livefyre.md)
      * [Facebook接続](./catalog/social/facebook.md)
      * [linkedIn Matched Audiences 接続](./catalog/social/linkedin.md)
      * [[!DNL Twitter Custom Audiences] 接続](./catalog/social/twitter.md)
   * ストリーミング先 {#streaming}
      * [ （ベータ版）HTTP API 接続](./catalog/streaming/http-destination.md)
   * 調査先 {#survey}
      * [調査先の概要](./catalog/survey/overview.md)
      * [Forese 拡張機能の宛先](./catalog/survey/foresee.md)
      * [InMoment 拡張機能](./catalog/survey/inmoment.md)
      * [Qualtrics Website Feedback 拡張機能](./catalog/survey/qualtrics.md)
      * [QuestionPro Intercept Surveys 拡張機能](./catalog/survey/web-intercept-surveys.md)
   * 顧客の声の宛先 {#voice}
      * [顧客の声の宛先の概要](./catalog/voice/overview.md)
      * [Confirmit Digital Feedback 拡張機能](./catalog/voice/confirmit-digital-feedback.md)
      * [Invoca Tags 拡張機能](./catalog/voice/invoca.md)
      * [Medallia 拡張機能](./catalog/voice/medallia.md)
      * [Talk URL Inbox 拡張機能](./catalog/voice/talkurl.md)
* 宛先 SDK {#destination-sdk}
   * [概要](./destination-sdk/overview.md)
   * [統合の前提条件](./destination-sdk/integration-prerequisites.md)
   * [はじめに](./destination-sdk/getting-started.md)
   * Destination SDK機能 {#functionality}
      * [設定オプション](./destination-sdk/configuration-options.md)
      * [宛先の設定](./destination-sdk/destination-configuration.md)
      * [サーバーとテンプレートの仕様](./destination-sdk/server-and-template-configuration.md)
      * [メッセージのフォーマット](./destination-sdk/message-format.md)
      * [Audience metadata management](./destination-sdk/audience-metadata-management.md)
      * 認証 {#authentication}
         * [認証設定](./destination-sdk/authentication-configuration.md)
         * [OAuth 2 認証](./destination-sdk/oauth2-authentication.md)
      * 開発者ツール {#developer-tools}
         * [メッセージ変換テンプレートの作成とテスト](./destination-sdk/create-template.md)
         * [宛先設定のテスト](./destination-sdk/test-destination.md)
   * API 操作 {#api}
      * [Destination SDK（宛先オーサリング）API リファレンス](https://www.adobe.io/experience-platform-apis/references/destination-authoring/)
      * [宛先エンドポイント API 操作](./destination-sdk/destination-configuration-api.md)
      * [宛先サーバーエンドポイント API の操作](./destination-sdk/destination-server-api.md)
      * [オーディエンスメタデータエンドポイント API の操作](./destination-sdk/audience-metadata-api.md)
      * [資格情報エンドポイント API 操作](./destination-sdk/credentials-configuration-api.md)
      * [公開エンドポイント API の操作](./destination-sdk/destination-publish-api.md)
      * 開発者ツールリファレンス {#developer-tools-reference}
         * [「Get sample template API」操作](./destination-sdk/sample-template-api.md)
         * [レンダリングテンプレート API 操作](./destination-sdk/render-template-api.md)
         * [宛先テスト API 操作](./destination-sdk/destination-testing-api.md)
         * [プロファイル生成 API 操作の例](./destination-sdk/sample-profile-generation-api.md)
   * ガイド {#guides}
      * [Destination SDKを使用したストリーミング先の設定](./destination-sdk/configure-destination-instructions.md)
      * [送信してレビュー用に、Destination SDKで作成した宛先を送信](./destination-sdk/submit-destination.md)
   * 宛先のドキュメント化 {#document-destination}
      * [Adobe Experience Platformでの宛先のドキュメント化](./destination-sdk/docs-framework/documentation-instructions.md)
      * [GitHub Web インターフェイスを使用して、宛先のドキュメントページを作成します](./destination-sdk/docs-framework/use-github-interface-to-create-documentation.md)
      * [ローカル環境でテキストエディターを使用して、宛先のドキュメントページを作成する](./destination-sdk/docs-framework/work-in-local-environment.md)
      * [ドキュメントのセルフサービステンプレート](./destination-sdk/docs-framework/self-service-template.md)
* [よくある質問](./destinations-faq.md)
* [Platform リリースノート](https://docs.adobe.com/content/help/ja-JP/experience-platform/release-notes/latest.html)
