---
audience: user
user-guide-title: 宛先ガイド
user-guide-description: クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告など、多くのユースケースで既知および未知のデータを有効化します。
description: このドキュメントでは、Adobe Experience Platform の宛先の目次を示します
feature: Destinations
source-git-commit: 7f6c949851888645a70f15f091397be0b96c76d7
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 74%

---


# 宛先 {#destinations}

* [宛先の概要](./home.md)
* [宛先のタイプとカテゴリ](./destination-types.md)
* API チュートリアル {#api}
   * [Flow Service API でストリーミング宛先に接続してデータを有効化する](./api/streaming-destinations.md)
   * [Flow Service API でクラウドストレージやメールマーケティングのバッチ宛先に接続してデータを有効化する](./api/connect-activate-batch-destinations.md)
   * [（ベータ版）アドホックなアクティベーション API を介してバッチ宛先に対してオーディエンスセグメントを有効化する](./api/ad-hoc-activation-api.md)
   * [宛先データフローの更新](./api/update-destination-dataflows.md)
   * [宛先アカウントの削除](./api/delete-destination-account.md)
   * [宛先データフローの削除](./api/delete-destination-dataflow.md)
* UI ガイド {#ui}
   * [宛先ワークスペース](./ui/destinations-workspace.md)
   * [新しい宛先接続の作成](./ui/connect-destination.md)
   * 宛先へのオーディエンスデータの有効化 {#activate}
      * [有効化の概要](./ui/activation-overview.md)
      * [ストリーミングセグメント書き出し宛先に対するオーディエンスデータの有効化](./ui/activate-segment-streaming-destinations.md)
      * [ストリーミングプロファイル書き出し宛先に対するオーディエンスデータの有効化](./ui/activate-streaming-profile-destinations.md)
      * [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](./ui/activate-batch-profile-destinations.md)
      * [プロファイルリクエスト宛先に対するオーディエンスデータの有効化](./ui/activate-profile-request-destinations.md)
      * [同じページと次のページのパーソナライズのためのパーソナライズ機能宛先の設定](./ui/configure-personalization-destinations.md)
      * [（ベータ版）Experience PlatformUI を使用して、オンデマンドでバッチ保存先にファイルを書き出す](./ui/export-file-now.md)
   * [宛先の詳細を表示](./ui/destination-details-page.md)
   * [宛先アカウントの更新](./ui/update-accounts.md)
   * [宛先アカウントの削除](./ui/delete-destination-account.md)
   * [アクティベーションデータフローを編集](./ui/edit-activation.md)
   * [宛先の削除](./ui/delete-destinations.md)
   * [データフローのモニタリング](./ui/monitor-dataflows.md)
   * [コンテキスト内宛先アラートを購読](ui/alerts.md)
* 宛先カタログ {#catalog}
   * [宛先カタログの概要](./catalog/overview.md)
   * アドビの宛先 {#adobe}
      * [アドビの宛先の概要](./catalog/adobe/overview.md)
      * [Marketo Engage 接続](./catalog/adobe/marketo-engage.md)
      * [Experience Platform セグメントの共有](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html?lang=ja)
   * 広告の宛先 {#advertising}
      * [広告の宛先の概要](./catalog/advertising/overview.md)
      * [Adobe Advertising Cloud接続](./catalog/advertising/adobe-advertising-cloud-connection.md)
      * [Adobe Advertising Cloud 拡張機能](./catalog/advertising/adobe-advertising-cloud.md)
      * [Awin Advertiser Conversion Tag 拡張機能](./catalog/advertising/awin-conversiontag.md)
      * [Awin Advertiser Mastertag 拡張機能](./catalog/advertising/awin-mastertag.md)
      * [Bing Ads Universal Event Tracking（UET）拡張機能](./catalog/advertising/bing-ads.md)
      * [Branch 拡張機能](./catalog/advertising/branch.md)
      * [（ベータ版）Criteo 接続](./catalog/advertising/criteo.md)
      * [DoubleClick Floodlight（ベータ版）拡張機能](./catalog/advertising/doubleclick-floodlight.md)
      * [Facebook Pixel 拡張機能](./catalog/advertising/facebook-pixel.md)
      * [Flashtalking OneTag 拡張機能](./catalog/advertising/flashtalking.md)
      * [Google Ads 接続](./catalog/advertising/google-ads-destination.md)
      * [Google Ads 拡張機能](./catalog/advertising/google-ads-extension.md)
      * [Google Ad Manager の接続](./catalog/advertising/google-ad-manager.md)
      * [（ベータ版）Google Ad Manager 360 接続](./catalog/advertising/google-ad-manager-360-connection.md)
      * [Google Customer Match 接続](./catalog/advertising/google-customer-match.md)
      * [Google Display &amp; Video 360 接続](./catalog/advertising/google-dv360.md)
      * [Google gtag 拡張機能](./catalog/advertising/gtag-advertising.md)
      * [LinkedIn Insight Tag 拡張機能](./catalog/advertising/linkedin.md)
      * [Microsoft Bing 接続](./catalog/advertising/bing.md)
      * [Pinterest Conversion Tracking 拡張機能](./catalog/advertising/pinterest-extension.md)
      * [Pinterest Customer List 接続](./catalog/advertising/pinterest.md)
      * [（ベータ版）Snapchat 広告接続](./catalog/advertising/snap-inc.md)
      * [Trade Desk 接続](./catalog/advertising/tradedesk.md)
      * [（ベータ版）トレードデスク CRM 接続](./catalog/advertising/tradedesk-emails.md)
      * [Twitter Universal Website Tag 拡張機能](./catalog/advertising/twitter-uwt.md)
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
      * [Yext Conversion Tracking 拡張機能](./catalog/analytics/yext.md)
   * クラウドストレージの宛先 {#cloud-storage}
      * [クラウドストレージの宛先の概要](./catalog/cloud-storage/overview.md)
      * [Amazon Kinesis接続](./catalog/cloud-storage/amazon-kinesis.md)
      * [Amazon S3 接続](./catalog/cloud-storage/amazon-s3.md)
      * [Azure Blob 接続](./catalog/cloud-storage/azure-blob.md)
      * [Azure Event Hubs 接続](./catalog/cloud-storage/azure-event-hubs.md)
      * [SFTP 接続](./catalog/cloud-storage/sftp.md)
      * [クラウドストレ許可リストージの宛先の IP アドレス](./catalog/cloud-storage/ip-address-allow-list.md)
   * 顧客関係管理 (CRM) の宛先 {#crm}
      * [Salesforce CRM 接続](./catalog/crm/salesforce.md)
      * [アウトリーチ接続](catalog/crm/outreach.md)
   * データ管理プラットフォームの宛先 {#data-management}
      * [データ管理プラットフォーム（DMP）の宛先の概要](./catalog/data-management/overview.md)
      * [Audience Manager DIL 拡張機能](./catalog/data-management/aam-dil-extension.md)
   * メールの宛先 {#email}
      * [Bizible 拡張機能](./catalog/email/bizible.md)
      * [Marketo 拡張機能](./catalog/email/marketo.md)
      * [Marketo Munchkin 拡張機能](./catalog/email/marketo-munchkin.md)
      * [PebblePost 拡張機能](./catalog/email/pebblepost.md)
   * メールマーケティングの宛先 {#email-marketing}
      * [メールマーケティングの宛先の概要](./catalog/email-marketing/overview.md)
      * [Adobe Campaign 接続](./catalog/email-marketing/adobe-campaign.md)
      * [Oracle Eloqua 接続](./catalog/email-marketing/oracle-eloqua.md)
      * [Oracle Responsys 接続](./catalog/email-marketing/oracle-responsys.md)
      * [(API)SalesforceMarketing Cloud接続](./catalog/email-marketing/salesforce-marketing-cloud-exact-target.md)
      * [（ファイル） SalesforceMarketing Cloud接続](./catalog/email-marketing/salesforce-marketing-cloud.md)
      * [SendGrid 接続](./catalog/email-marketing/sendgrid.md)
   * タグ拡張機能 {#launch-extensions}
      * [タグ拡張機能の概要](./catalog/launch-extensions/overview.md)
   * モバイルエンゲージメントの宛先 {#mobile-engagement}
      * [モバイルエンゲージメントの宛先の概要](./catalog/mobile-engagement/overview.md)
      * [Airship Attributes 接続](./catalog/mobile-engagement/airship-attributes.md)
      * [Airship Tags 接続](./catalog/mobile-engagement/airship-tags.md)
      * [Braze 接続](./catalog/mobile-engagement/braze.md)
   * パーソナライゼーションの宛先 {#personalization}
      * [パーソナライゼーションの宛先の概要](./catalog/personalization/overview.md)
      * [Adobe Target 接続](./catalog/personalization/adobe-target-connection.md)
      * [Adobe Target 拡張機能](./catalog/personalization/adobe-target.md)
      * [Adobe Target v2 拡張機能](./catalog/personalization/adobe-target-v2.md)
      * [Beemray 拡張機能](./catalog/personalization/beemray.md)
      * [カスタムパーソナライゼーション接続](./catalog/personalization/custom-personalization.md)
      * [D&amp;B 訪問者インテリジェンス拡張機能](./catalog/personalization/dnb.md)
      * [Experience Cloud ID サービス拡張機能](./catalog/personalization/adobe-ecid.md)
      * [Gainsight 拡張機能](./catalog/personalization/gainsight.md)
      * [KickFire 拡張機能](./catalog/personalization/kickfire.md)
      * [Marketo web パーソナライゼーション拡張機能](./catalog/personalization/marketo-web-personalization.md)
      * [Pega Customer Decision Hub 接続](./catalog/personalization/pega.md)
   * ソーシャルの宛先 {#social}
      * [ソーシャルの宛先の概要](./catalog/social/overview.md)
      * [Adobe Livefyre 拡張機能](./catalog/social/adobe-livefyre.md)
      * [Facebook 接続](./catalog/social/facebook.md)
      * [linkedIn Matched Audiences 接続](./catalog/social/linkedin.md)
      * [[!DNL Twitter Custom Audiences] 接続](./catalog/social/twitter.md)
   * ストリーミングの宛先 {#streaming}
      * [HTTP API 接続](./catalog/streaming/http-destination.md)
      * [ストリーミング許可リスト先の IP アドレス](./catalog/streaming/ip-address-allow-list.md)
   * サーベイの宛先 {#survey}
      * [サーベイの宛先の概要](./catalog/survey/overview.md)
      * [Foresee 拡張機能の宛先](./catalog/survey/foresee.md)
      * [InMoment 拡張機能](./catalog/survey/inmoment.md)
      * [Qualtrics Website Feedback 拡張機能](./catalog/survey/qualtrics.md)
      * [QuestionPro Intercept Surveys 拡張機能](./catalog/survey/web-intercept-surveys.md)
   * お客様の声の宛先 {#voice}
      * [お客様の声の宛先の概要](./catalog/voice/overview.md)
      * [Confirmit Digital Feedback 拡張機能](./catalog/voice/confirmit-digital-feedback.md)
      * [Invoca Tags 拡張機能](./catalog/voice/invoca.md)
      * [Medallia 接続](./catalog/voice/medallia-connector.md)
      * [Medallia 拡張機能](./catalog/voice/medallia.md)
      * [Talk URL Inbox 拡張機能](./catalog/voice/talkurl.md)
* Destination SDK {#destination-sdk}
   * [概要](./destination-sdk/overview.md)
   * [統合の前提条件](./destination-sdk/integration-prerequisites.md)
   * [はじめに](./destination-sdk/getting-started.md)
   * Destination SDK の機能 {#functionality}
      * [設定オプション](./destination-sdk/configuration-options.md)
      * [ストリーミング先の構成](./destination-sdk/destination-configuration.md)
      * [（ベータ版）ファイルベースの宛先設定](./destination-sdk/file-based-destination-configuration.md)
      * [ストリーミング宛先のサーバーとテンプレートの仕様](./destination-sdk/server-and-template-configuration.md)
      * [（ベータ版）ファイルベースの宛先のサーバーとファイルの仕様](./destination-sdk/server-and-file-configuration.md)
      * [メッセージの形式](./destination-sdk/message-format.md)
      * [オーディエンスメタデータの管理](./destination-sdk/audience-metadata-management.md)
      * 認証 {#authentication}
         * [認証設定](./destination-sdk/authentication-configuration.md)
         * [OAuth 2 認証](./destination-sdk/oauth2-authentication.md)
      * デベロッパーツール {#developer-tools}
         * [メッセージ変換テンプレートの作成とテスト](./destination-sdk/create-template.md)
         * [宛先設定のテスト](./destination-sdk/test-destination.md)
   * API 操作 {#api}
      * [Destination SDK（宛先オーサリング）API リファレンス](https://www.adobe.io/experience-platform-apis/references/destination-authoring/)
      * [宛先エンドポイント API の操作](./destination-sdk/destination-configuration-api.md)
      * [宛先サーバーエンドポイント API の操作](./destination-sdk/destination-server-api.md)
      * [オーディエンスメタデータエンドポイント API の操作](./destination-sdk/audience-metadata-api.md)
      * [資格情報エンドポイント API の操作](./destination-sdk/credentials-configuration-api.md)
      * [公開エンドポイント API の操作](./destination-sdk/destination-publish-api.md)
      * デベロッパーツールリファレンス {#developer-tools-reference}
         * ストリーミング宛先テスト API {#streaming-destination-testing-api}
            * [サンプルテンプレート取得 API の操作](./destination-sdk/sample-template-api.md)
            * [テンプレートレンダリング API の操作](./destination-sdk/render-template-api.md)
            * [宛先テスト API の操作](./destination-sdk/destination-testing-api.md)
            * [サンプルプロファイル生成 API の操作](./destination-sdk/sample-profile-generation-api.md)
         * ファイルベースの宛先テスト API {#file-based-destination-testing-api}
            * [ファイルベースの宛先テスト API の概要](./destination-sdk/file-based-destination-testing-overview.md)
            * [ソーススキーマに基づくサンプルプロファイルの生成](./destination-sdk/file-based-sample-profile-generation-api.md)
            * [サンプルプロファイルを使用してファイルベースの宛先をテストする](./destination-sdk/file-based-destination-testing-api.md)
            * [詳細なアクティベーション結果の表示](./destination-sdk/file-based-destination-results-api.md)
            * [テンプレート化された顧客フィールドの検証](./destination-sdk/file-based-render-template-api.md)
   * ガイド {#guides}
      * [Destination SDK を使用したストリーミングの宛先の設定](./destination-sdk/configure-destination-instructions.md)
      * [（ベータ版）Destination SDK を使用したファイルベースの宛先の設定](./destination-sdk/configure-file-based-destination-instructions.md)
      * [Destination SDK で作成した宛先のレビュー用に送信する](./destination-sdk/submit-destination.md)
      * ファイルベースの宛先の設定 {#configure-file-based-destinations}
         * [（ベータ版）事前に定義されたファイル形式オプションとカスタムファイル名設定を使用して、Amazon S3 の宛先を設定する](../destinations/destination-sdk/guides/batch/configure-amazon-s3-destination-with-predefined-file-formatting.md)
         * [（ベータ版）カスタムのファイル名と書式設定オプションを使用してAmazon S3 の宛先を設定する](../destinations/destination-sdk/guides/batch/configure-amazon-s3-destination-with-custom-file-formatting.md)
         * [（ベータ版）カスタムのファイルフォーマットオプションとカスタムのファイル名設定を使用して Azure Blob ストレージの宛先を設定する](../destinations/destination-sdk/guides/batch/configure-blob-destination-with-custom-file-formatting.md)
         * [（ベータ版）カスタムのファイル形式設定オプションとカスタムのファイル名設定を使用して Azure Data Lake Storage の宛先を設定する](../destinations/destination-sdk/guides/batch/configure-adls-destination-with-custom-file-formatting.md)
         * [（ベータ版）カスタムのファイル形式設定オプションとカスタムのファイル名設定を使用して、データランディングゾーン (DLZ) の宛先を構成する](../destinations/destination-sdk/guides/batch/configure-dlz-destination-with-custom-file-formatting.md)
         * [（ベータ版）事前に定義されたファイル形式オプションとカスタムのファイル名設定を使用して SFTP の宛先を設定する](../destinations/destination-sdk/guides/batch/configure-sftp-destination-with-predefined-file-formatting.md)
   * リファレンス {#reference}
      * [ストリーミング宛先のレート制限および再試行ポリシー](./destination-sdk/rate-limiting-retry-policy.md)
      * [サポートされる変換関数](./destination-sdk/supported-functions.md)
   * 宛先のドキュメント化 {#document-destination}
      * [Adobe Experience Platform の宛先のドキュメント化](./destination-sdk/docs-framework/documentation-instructions.md)
      * [GitHub web インターフェイスを使用した宛先ドキュメントページの作成](./destination-sdk/docs-framework/use-github-interface-to-create-documentation.md)
      * [ローカル環境でのテキストエディターを使用した宛先ドキュメントページの作成](./destination-sdk/docs-framework/work-in-local-environment.md)
      * [ドキュメントのセルフサービステンプレート](./destination-sdk/docs-framework/self-service-template.md)
      * [オーサリングのベストプラクティス](./destination-sdk/docs-framework/authoring-best-practices.md)
* [よくある質問](./destinations-faq.md)
* [Platform リリースノート](https://docs.adobe.com/content/help/ja-JP/experience-platform/release-notes/latest.html)
