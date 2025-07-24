---
product: adobe experience platform
solution: Real-Time Customer Data Platform
audience: user
user-guide-title: Real-time Customer Data Platform ガイド
user-guide-description: 複数のエンタープライズソースから既知のデータや匿名データをまとめて顧客プロファイルを作成、それらのプロファイルからオーディエンスを作成し、それらのオーディエンスをサードパーティの宛先に活用します。
role: Admin
source-git-commit: 74a73b568c850f8e749afea039afd2821858bd69
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 60%

---


# Real-time Customer Data Platform のヘルプ {#rtcdp}

* [Real-Time CDP ドキュメント](home.md)
* 基本を学ぶ {#intro}
   * Real-Time CDP {#rtcdp-intro}
      * [Real-Time CDP の概要](overview.md)
      * [Real-Time CDP の基本を学ぶ](get-started.md)
      * [ホームページ](home-page-dashboards.md)
   * Real-Time CDP B2B エディション {#rtcdpb2b-intro}
      * [Real-Time CDP B2B Edition の概要](b2b-overview.md)
      * [使用例](./b2b-use-case.md)
      * [エンドツーエンドのチュートリアル](./b2b-tutorial.md)
      * [Real-Time CDP B2B Edition のガードレール](b2b-guardrails.md)
      * [Real-Time CDP B2B edition アーキテクチャのアップグレード](b2b-architecture-upgrade.md)
* Audience ManagerとReal-Time CDP {#evolution}
   * [Audience Manager からの進化](aam-to-rtcdp.md)
* アカウントプロファイル {#account}
   * [アカウントプロファイルの概要](accounts/account-profile-overview.md)
   * [アカウントプロファイル UI ガイド](accounts/account-profile-ui-guide.md)
* 管理 {#admin}
   * [管理の概要](administration/admin-overview.md)
* オーディエンスとセグメント化 {#segmentation}
   * [セグメント化の概要](segmentation/segmentation-overview.md)
   * [Audience Builder ガイド](segmentation/audience-builder.md)
   * [Real-Time CDP B2B Edition のセグメント化](segmentation/b2b.md)
   * [顧客 AI](segmentation/customer-ai.md)
* データセット {#datasets}
   * [データセット](datasets/dataset.md)
   * [Experience Platformのデータ品質](datasets/data-quality.md)
* 宛先 {#destinations}
   * [宛先の概要](destinations/overview.md)
   * [Real-Time CDP B2B Edition の宛先](destinations/b2b.md)
* ガードレール {#guardrails}
   * [Real-Time CDP ガードレールの概要](guardrails/overview.md)
   * [ データ取り込みのガードレール](https://experienceleague.adobe.com/docs/experience-platform/ingestion/guardrails.html){target="_blank"}
   * [ のガードレール  [!DNL Edge Network API]](https://developer.adobe.com/data-collection-apis/docs/getting-started/guardrails/){target="_blank"}
   * [ データとセグメ  [!DNL Real-Time Customer Profile]  ト化のガードレール](https://experienceleague.adobe.com/docs/experience-platform/profile/guardrails.html?lang=ja){target="_blank"}
   * [ データのガ  [!DNL Identity Service]  ドレール](https://experienceleague.adobe.com/docs/experience-platform/identity/guardrails.html){target="_blank"}
   * [ のガードレール  [!DNL Query Service]](https://experienceleague.adobe.com/docs/experience-platform/query/guardrails.html){target="_blank"}
   * [ 宛先を介したデータのアクティベーションのためのガードレール](https://experienceleague.adobe.com/docs/experience-platform/destinations/guardrails.html){target="_blank"}
* ID {#identity}
   * [ID と ID 名前空間](profile/identities-overview.md)
* 結合ポリシー {#merge-policies}
   * [結合ポリシーの概要](profile/merge-policies.md)
* プライバシーとデータガバナンス {#privacy}
   * [プライバシーの概要](privacy/privacy-overview.md)
   * [データガバナンスの概要](privacy/data-governance-overview.md)
* プロファイル {#profile}
   * [プロファイルの概要](profile/profile-overview.md)
   * [プロファイルの参照](profile/profile-browse.md)
* Real-Time CDP B2B editionの AI/ML サービス {#b2b-cdp-ai-ml}
   * [関連するアカウント](b2b-ai-ml-services/related-accounts.md)
   * [リードとアカウントのマッチング](b2b-ai-ml-services/lead-to-account-matching.md)
   * リードおよびアカウントの予測スコアリング {#predictive-lead-and-account-scoring-intro}
      * [リードおよびアカウントの予測スコアリングの概要](b2b-ai-ml-services/predictive-lead-and-account-scoring.md)
      * [リードおよびアカウントの予測スコアリングの管理](b2b-ai-ml-services/manage-predictive-lead-and-account-scoring.md)
* スキーマ {#schemas}
   * [スキーマの概要](schemas/overview.md)
   * [Real-Time CDP B2B Edition のスキーマ](schemas/b2b.md)
* ソース {#sources}
   * [ソースの概要](sources/sources-overview.md)
   * [Real-Time CDP B2B Edition のソース](sources/b2b.md)
* ユースケース {#use-cases}
   * [サンプルユースケースの概要](/help/rtcdp/use-case-guides/overview.md)
   * 顧客獲得 {#customer-acquisition}
      * [ サードパーティ cookie に依存せずに新規顧客を獲得および獲得 ](/help/rtcdp/partner-data/prospecting.md)
      * [パートナー支援の訪問者認識を使用して、不明な訪問者に対するオンサイトエクスペリエンスをパーソナライズする](/help/rtcdp/partner-data/onsite-personalization.md)
      * [認証されていないユーザーのオフサイトリターゲティング](./partner-data/offsite-retargeting.md)
      * [認証されていないサーバーサイドリターゲティング](./partner-data/unauthenticated-retargeting.md)
   * プロファイルエンリッチメント {#profile-enrichment}
      * [パートナー提供の属性を使用してファーストパーティプロファイルを補完](/help/rtcdp/partner-data/supplement-first-party-profiles.md)
   * パーソナライズされたインサイトとエンゲージメント {#personalization-insights-engagement}
      * [1 回限りの顧客価値をライフタイム価値に進化](/help/rtcdp/use-case-guides/evolve-one-time-value-lifetime-value/evolve-one-time-value-to-lifetime-value.md)
      * [お客様をインテリジェントに再エンゲージ](/help/rtcdp/use-case-guides/intelligent-re-engagement/intelligent-re-engagement.md)
      * [顧客をインテリジェントに再エンゲージ：Luma の例](/help/rtcdp/use-case-guides/intelligent-re-engagement/use-cases-luma.md)
* [Experience Platform リリースノート](https://experienceleague.adobe.com/ja/docs/experience-platform/release-notes/latest)
* [Experience Platform の用語](https://docs.adobe.com/content/help/ja-JP/experience-platform/landing/glossary.html)