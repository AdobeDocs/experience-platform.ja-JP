---
product: adobe experience platform
solution: Real-Time Customer Data Platform
audience: user
user-guide-title: Real-time Customer Data Platform ガイド
user-guide-description: 複数のエンタープライズソースからの既知データや匿名データをまとめて顧客プロファイルを作成し、それらのプロファイルからオーディエンスセグメントを作成して、それらのセグメントをサードパーティの宛先に対してアクティブ化します。
source-git-commit: 98171126ea09ec9e81daeeb9122da8a44ab8c16b
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 74%

---


# Real-time Customer Data Platform のヘルプ {#rtcdp}

* [Real-Time CDP の概要](overview.md)
* [Real-Time CDP B2B Edition の概要](b2b-overview.md)
* はじめに {#intro}
   * Real-Time CDP {#rtcdp-intro}
      * [Real-Time CDP の基本を学ぶ](get-started.md)
      * [ホームページ](home-page-dashboards.md)
   * Real-Time CDP B2B Edition {#rtcdpb2b-intro}
      * [使用例](./b2b-use-case.md)
      * [エンドツーエンドのチュートリアル](./b2b-tutorial.md)
      * [Real-Time CDP B2B Edition のガードレール](b2b-guardrails.md)
* Audience ManagerとReal-Time CDP {#evolution}
   * [Audience Manager からの進化](aam-to-rtcdp.md)
* アカウントプロファイル {#account}
   * [アカウントプロファイルの概要](accounts/account-profile-overview.md)
   * [アカウントプロファイル UI ガイド](accounts/account-profile-ui-guide.md)
* 管理 {#admin}
   * [管理の概要](administration/admin-overview.md)
* データセット {#datasets}
   * [データセット](datasets/dataset.md)
   * [プラットフォーム上のデータ品質](datasets/data-quality.md)
* 宛先 {#destinations}
   * [宛先の概要](destinations/overview.md)
   * [Real-Time CDP B2B Edition の宛先](destinations/b2b.md)
* ガードレール {#guardrails}
   * [Real-Time CDP Guardrails の概要](guardrails/overview.md)
   * [データ取り込みのガードレール](https://experienceleague.adobe.com/docs/experience-platform/ingestion/guardrails.html){target="_blank"}
   * [のガードレール [!DNL Edge Network Server API]](https://experienceleague.adobe.com/docs/experience-platform/edge-network-server-api/guardrails.html){target="_blank"}
   * [のガードレール [!DNL Real-Time Customer Profile] データとセグメント化](https://experienceleague.adobe.com/docs/experience-platform/profile/guardrails.html?lang=ja){target="_blank"}
   * [ [!DNL Identity Service] データ](https://experienceleague.adobe.com/docs/experience-platform/identity/guardrails.html)のガードレール{target="_blank"}
   * [のガードレール [!DNL Query Service]](https://experienceleague.adobe.com/docs/experience-platform/query/guardrails.html){target="_blank"}
   * [宛先を介したデータのアクティベーションのガードレール](https://experienceleague.adobe.com/docs/experience-platform/destinations/guardrails.html){target="_blank"}
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
* Real-Time CDP B2B Edition の AI／ML サービス {#b2b-cdp-ai-ml}
   * [関連するアカウント](b2b-ai-ml-services/related-accounts.md)
   * [リードとアカウントのマッチング](b2b-ai-ml-services/lead-to-account-matching.md)
   * リードおよびアカウントの予測スコアリング {#predictive-lead-and-account-scoring-intro}
      * [リードおよびアカウントの予測スコアリングの概要](b2b-ai-ml-services/predictive-lead-and-account-scoring.md)
      * [リードおよびアカウントの予測スコアリングの管理](b2b-ai-ml-services/manage-predictive-lead-and-account-scoring.md)
* スキーマ {#schemas}
   * [スキーマの概要](schemas/overview.md)
   * [Real-Time CDP B2B Edition のスキーマ](schemas/b2b.md)
* セグメント {#segmentation}
   * [セグメント化の概要](segmentation/segmentation-overview.md)
   * [セグメントビルダーガイド](segmentation/segment-builder-guide.md)
   * [Real-Time CDP B2B Edition のセグメント化](segmentation/b2b.md)
   * [顧客 AI](segmentation/customer-ai.md)
* ソース {#sources}
   * [ソースの概要](sources/sources-overview.md)
   * [Real-Time CDP B2B Edition のソース](sources/b2b.md)
* ユースケース {#use-cases}
   * パーソナライゼーション、インサイト、エンゲージメントの使用例 {#personalization-insights-engagement}
      * [顧客をインテリジェントに再び関与](/help/rtcdp/use-case-guides/intelligent-re-engagement/intelligent-re-engagement.md)
   * 顧客獲得 {#customer-acquisition}
      * [サードパーティ cookie に依存せずに新規顧客を惹きつけ、獲得する](/help/rtcdp/partner-data/prospecting.md)
      * [パートナー支援による訪問者認識を使用して、不明な訪問者に対するオンサイトエクスペリエンスをパーソナライズ](/help/rtcdp/partner-data/onsite-personalization.md)
      * [未認証ユーザーのオフサイトリターゲティング](./partner-data/offsite-retargeting.md)
   * プロファイルエンリッチメント {#profile-enrichment}
      * [パートナー提供の属性を使用してファーストパーティプロファイルを補完](/help/rtcdp/partner-data/supplement-first-party-profiles.md)
* [Experience Platform リリースノート](https://docs.adobe.com/content/help/ja-JP/experience-platform/release-notes/latest.html)
* [Experience Platform の用語](https://docs.adobe.com/content/help/ja-JP/experience-platform/landing/glossary.html)