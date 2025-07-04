---
keywords: Experience Platform；ホーム；人気のトピック；GDPR;gdpr;CCPA;ccpa;PDPA;pdpa;LGPD;lgpd；概要；概要；規制；規制；規制；プライバシー；プライバシー；
solution: Experience Platform
title: プライバシー規制の概要
description: このドキュメントでは、Adobe Experience Cloudでサポートされている様々なプライバシー規制の概要について説明します。
exl-id: 2ca946cf-94f8-4fd8-bb1a-7f06a5ab1256
source-git-commit: c2394035dd6bd4fe6dbb443e4db13934a27066a6
workflow-type: tm+mt
source-wordcount: '1920'
ht-degree: 0%

---

# プライバシー規制の概要

>[!IMPORTANT]
>
>増加する米国の州のプライバシー法をサポートするために、Privacy Serviceは `regulation_type` の価値観を変えています。 **2025 年 6 月 12 日（PT）** 以降は、州の略語（`ucpa_ut_usa` など）を含んだ新しい値を使用します。 古い値（例：`ucpa_usa`）は **2025 年 7 月 28 日（PT）** 以降は機能しなくなります。
>
>リクエストの失敗を避けるために、この期限の前に統合を更新してください。

このドキュメントでは、Adobe Experience Cloudでサポートされている様々なプライバシー規制の概要について説明します。

Experience Cloudでは、[Adobe Experience Platform Privacy Service](../home.md) を通じて、次の規則に基づくアクセスリクエストおよび削除リクエストをサポートしています。

| 規則 | `regulation_type` | 説明 |
|-----------|-------------------|-------------|
| APA （オーストラリア） | `apa_aus` | この [[!DNL Australia Privacy Act (Privacy Act)]](https://www.oaic.gov.au/privacy/the-privacy-act) は、個人のプライバシーを促進および保護し、オーストラリア政府機関および組織による個人情報の取り扱いを規制しています。 プライバシー法には、民間部門の組織に適用される原則が含まれています。 例えば、個人には、個人情報が収集される理由と使用方法、アクセス機能、データ消去および個人情報の訂正機能を理解する権利が与えられます。 |
| CCPA （カリフォルニア州） | `ccpa` | [[!DNL California Consumer Privacy Act (CCPA)]](https://oag.ca.gov/privacy/ccpa) は、米国カリフォルニア州の居住者のプライバシー権と消費者保護を強化します。 CCPA は、カリフォルニア州の居住者に新しいデータプライバシー権を提供します。 これには、個人データにアクセスして削除する権利、個人データが（誰に）販売または公開されているかどうかを知る権利、およびデータを第三者に販売することをオプトアウトする権利が含まれます。 |
| CPA （コロラド州） | `cpa_co_usa` | [[!DNL Colorado Privacy Act]](https://coag.gov/resources/colorado-privacy-act/) （CPA）は、個人データ管理者が何を収集、共有、販売するのか、およびそのデータがどのように使用されるかについて、コロラド州のコンシューマーに追加のinsightを提供します。 CPA は、コロラド州住民が個人または家庭内で行動する際に、その個人データを保護します。 これらのルールでは、1 つ以上のユニバーサルオプトアウトメカニズムの技術仕様について詳しく説明します。 これらのメカニズムは、ターゲット広告や個人データの販売を目的とした個人データの処理をオプトアウトするという、消費者の肯定的、自由に与えられた、明確な選択を明確に伝えます。 |
| CPRA （カリフォルニア州） | `cpra_ca_usa` | [[!DNL California Consumer Privacy Rights Act (CPRA)]](https://cppa.ca.gov/regulations/consumer_privacy_act.html) は、カリフォルニア州消費者プライバシー法（CCPA）の一部を拡張および改正しています。 CPRA は、消費者の権利を拡大し、機密性の高い個人情報のより広い定義を通じてカバーされるデータの種類を拡大することにより、カリフォルニア州の消費者データプライバシーに対する新しいベースラインを確立します。 さらに、CPRA は、データプライバシールールの実装と実施に特化した新しい機関であるカリフォルニアプライバシー保護機関を設立しました。 |
| CTDPA （コネチカット州） | `ctdpa_ct_usa` | [[!DNL Connecticut Data Privacy Act]](https://portal.ct.gov/AG/Sections/Privacy/The-Connecticut-Data-Privacy-Act) は、コネチカット州の住民のための包括的な消費者プライバシー法であり、個人データに対する一定の権利を住民に付与します。 また、個人データを処理するデータ管理者の責任とプライバシー保護基準も確立します。 CTDPA は、個人として、または世帯コンテキストで行動するコネチカット州の居住者を保護します。 CTDPA は、お客様に対し、お客様の個人データに対するアクセス、訂正、削除、コピーの取得、または販売のオプトアウト、処理、またはプロファイリングを行う権限を付与します。 |
| DPDPA （デラウェア州） | `dpdpa_de_usa` | [[!DNL Delaware Personal Data Privacy Act]](https://legis.delaware.gov/json/BillDetail/GenerateHtmlDocument?legislationId=140388&legislationTypeId=1&docTypeId=2&legislationName=HB154) （DPDPA）は、デラウェア州の居住者に、個人データの販売とターゲット広告のオプトアウトに対するアクセス、修正、削除の権利を付与します。 これは、少なくとも 35,000 人の消費者のデータを処理する企業や、10,000 人以上の消費者に影響を与えるデータ販売から売上高の 20% 以上を獲得する企業に適用されます。 この法律は、消費者のデータ保護の実施、消費者のリクエストに対するタイムリーな対応、違反に対する 60 日間の治療期間を義務付けており、法務省によって施行されています。 |
| FDBR （フロリダ州） | `fdbr_fl_usa` | [[!DNL Florida Digital Bill of Rights]](https://flsenate.gov/Session/Bill/2023/262/BillText/er/HTML) （FDBR）は、フロリダ州の住民に包括的なデータプライバシー権を提供します。 この法律により、個人は、自分の個人データにアクセスし、修正し、削除し、コピーを取得する権利が確保されます。 また、消費者の同意を得ない監視などのオンラインプラットフォームによる特定の行為を禁止し、明確なプライバシー通知やターゲット広告のための個人データの販売または処理をオプトアウトする機能など、データ慣行の透明性を必要とします。 FDBR はフロリダ州法務省に対し、これらの権利を実施し、違反に対して民事罰を科すことを認可している。 法律の下で、データ管理者は、データ主体のリクエストを受信してから 45 日以内にリクエストに対応する義務があります。 |
| GDPR （欧州連合） | `gdpr` | [[!DNL General Data Protection Regulation (GDPR)]](https://gdpr-info.eu) は、アクセス権や忘れられる権利など、欧州経済領域（EEA）のメンバーに対するいくつかの新しいデータプライバシー権を導入しました。 これらの権利は、お客様のビジネスによって個人データが収集された EEA に住むすべての人が、いつでもデータへのアクセスまたは削除をリクエストできることを意味します。<br><br> 英国（Brexit 後）には独自の UK-GDPR 規制があり、EEA 版と同じ権利を国民に提供しています。 |
| HIPAA （米国） | `hipaa_usa` | [[!DNL Health Insurance Portability and Accountability Act (HIPAA)]](https://www.hhs.gov/hipaa/index.html) は、医療の効率を向上させ、医療保険の携行性を向上させ、患者と医療保険メンバーのプライバシーを保護するために作成された米国連邦法です。 HIPAA の下では、個人は自分の情報にアクセスして修正し、医療記録や医療情報のコピーを取得する権利を有する。 対象となるエンティティおよび対象となるエンティティのビジネスアソシエートは、HIPAA 規制に従う必要があります。 |
| ICDPA （アイオワ） | `icdpa_ia_usa` | [[!DNL Iowa Consumer Data Protection Act]](https://www.legis.iowa.gov/legislation/BillBook?ga=90&ba=SF%20262) は、アイオワ州の住民に個人データへのアクセス、削除、販売のオプトアウトの権利を提供している。 これは、10 万人以上のアイオワ州の住民のデータを処理する企業や、個人データの販売から収益の 50% 以上を生み出す企業に適用されます。 ICDPA は個人情報の消費者管理を重視していますが、非営利団体や教育機関などの特定の組織を除外しています。 また、この法律は、罰則が適用される前に違反を修正するための 90 日間の硬化期間を提供します。 |
| LGPD （ブラジル） | `lgpd_bra` | この [[!DNL Lei Geral de Proteção de Dados (LGPD)]](https://gdpr.eu/gdpr-vs-lgpd/) は、ブラジルにおけるすべての個人または自然人の個人データの取り扱いを規制することを目的としています。 LGPD はブラジル国民に、自身の個人データにアクセスして削除する権利、自身の個人データが（誰に）販売または公開されているかどうかを知る権利、および自身のデータを第三者に販売することをオプトアウトする権利を与える。 |
| MCDPA （ミネソタ州） | `mcdpa_mn_usa` | [[!DNL Minnesota Consumer Data Privacy Act (MCDPA)]](https://www.house.mn.gov/comm/docs/C6hTV3TEt0W2vuhEMtczrQ.pdf) は、ミネソタ州の住民に対し、個人データへのアクセス、訂正、削除、およびコピーの取得の権利を提供している。 また、法的または同様に重要な効果をもたらす決定を促進するために、個人データの販売、ターゲット広告、プロファイリングをオプトアウトする権利を付与します。 同法は、データ管理者に対し、明確なプライバシー通知を提供し、データ保護評価を実施し、合理的なデータセキュリティ対策を維持する義務を課しています。 執行権限は、ミネソタ検事総長に付与されます。 |
| MCDPA （モンタナ） | `mcdpa_mt_usa` | [[!DNL Montana Consumer Data Privacy Act]](https://legiscan.com/MT/text/SB384/id/2791095) は、事業者が収集、共有、販売する個人データの内容と、その利用目的を知る権利を住民に与えます。 また、収集したデータを修正、削除、またはコピーを取得する機能も付与されます。 この法律は、50,000 を超える Montana の消費者のデータを処理する企業に適用されます。 同法は、生体情報や遺伝情報を含む機密性の高い個人データの保護を重視している。 |
| MHMDA （ワシントン州） | `mhmda_wa_usa` | [[!DNL Washington My Health My Data Act]](https://app.leg.wa.gov/RCW/default.aspx?cite=19.373&full=true) は、消費者の健康データに関するプライバシー権を強化します。 健康データの開示、消費者の同意、削除権を義務付け、許可なく健康データを販売することを禁止しています。 さらに同法は、医療施設の周辺でジオフェンスを使用することを違法にしている。 |
| NDPA （ネブラスカ州） | `ndpa_ne_usa` | [[!DNL Nebraska Data Protection Act]](https://nebraskalegislature.gov/FloorDocs/108/PDF/Slip/LB1074.pdf) は、ネブラスカ人に対し、自身の個人データに対する権利を提供しています。これには、アクセス、訂正、削除、販売のオプトアウトなどが含まれます。 この法律は、データ処理と個人情報の販売からの収益に関する特定の閾値を満たす企業に適用されます。 また、企業に対しては、適切なデータセキュリティ対策を実施すること、およびコンプライアンスに関する問題を解決するために罰則が科される 30 日間のキュア期間を義務付けています。 |
| ニュージーランド [!DNL Privacy Act] | `nzpa_nzl` | [ ニュージーランド  [!DNL Privacy Act]](https://www.privacy.org.nz/privacy-act-2020/privacy-principles/) は、ニュージーランドの市民や団体の個人情報の収集、使用、開示、保管、アクセスの方法を管理しています。 2020 年に同法の最新版は、これらのプライバシー法に大幅な更新を加えました。 この更新には、新たな犯罪、罰金の増加、データ侵害に対する通知の義務化、個人情報保護委員会の権限強化などが含まれている。 |
| NHDPA （ニューハンプシャー州） | `nhpa_nh_usa` | [[!DNL New Hampshire Privacy Act]](https://www.doj.nh.gov/sites/g/files/ehbemt721/files/inline-documents/sonh/data-privacy-faqs-revised_0.pdf) は、データのアクセス、削除、ポータビリティに関する消費者権を確立することにより、ニューハンプシャー州の住民の個人情報を保護します。 組織はデータ収集と共有の慣行を開示する必要があり、消費者はデータ販売をオプトアウトできます。 この法律は、特定のデータ処理しきい値を満たすビジネスに適用されます。 |
| NJDPA （ニュージャージー州） | `njdpa_nj_usa` | [[!DNL New Jersey Data Protection Act]](https://pub.njleg.state.nj.us/Bills/2022/S0500/332_R6.PDF) は、ニュージャージー州の居住者に情報へのアクセス、訂正、および削除の権利を提供することにより、自身の個人データを管理することを許可します。 データ販売とターゲット広告のオプトアウトメカニズムが含まれます。 この法律は、大量の消費者データを処理し、データ使用の透明性を義務付ける企業を対象としています。 |
| OCPA （オレゴン） | `ocpa_or_usa` | [[!DNL Oregon Consumer Privacy Act]](https://olis.oregonlegislature.gov/liz/2023R1/Downloads/PublicTestimonyDocument/59856#:~:text=The%20Act%20requires%20controllers%20to,data%3B%20and%20%E2%80%A2%20Contact%20information.) （OCPA）は、オレゴンの居住者に個人データの基本的権利を提供し、そのようなデータを処理する企業に義務を課します。 消費者は、自分のデータのコピーを知り、修正し、削除し、取得する権利を持ち、ターゲット広告や販売のためのデータ処理をオプトアウトします。 同法は、機密データの保護強化、特定の目的を超えたデータ処理に対する同意、データ管理者による包括的なプライバシー通知の提供を義務付けています。 |
| PDPA （タイ） | `pdpa_tha` | この [[!DNL Personal Data Protection Act (PDPA)]](https://www.pdpc.gov.sg/Overview-of-PDPA/The-Legislation/Personal-Data-Protection-Act) は、タイのデータ所有者を個人データの違法な収集、使用、開示から保護するために導入されました。 欧州連合（EU）の GDPR に触発されたこの規制は、タイ国民に対し、保存されている個人データへのアクセスまたは削除をリクエストする権利を付与します。 |
| ql25 （ケベック州） | `ql25_qc_can` | [[!DNL Quebec Law 25]](https://www.canlii.org/en/qc/laws/astat/sq-2021-c-25/latest/sq-2021-c-25.html) （QL25）は、ケベックの居住者のプライバシー権を強化し、グローバル標準に準拠します。 同法は、居住者に対し、個人データへのアクセス、訂正、削除および移転について、明示的な同意、データの最小化および権利を義務付けている。 また、組織は、プライバシー責任者を任命し、プライバシー影響評価を実施し、違反を通知する必要があります。 法的に適用されるコンプライアンス期限と、コンプライアンス違反に対する相当の罰則が適用されます。 |
| TDPSA （テキサス州） | `tdpsa_tx_usa` | [[!DNL Texas Data Privacy and Security Act]](https://capitol.texas.gov/BillLookup/Text.aspx?LegSess=88R&Bill=HB4) （TDPSA）は、テキサス州における消費者の個人データの収集、使用、処理、および処理を規制します。 2024 年 7 月 1 日より、データにアクセス、修正、削除、コピーの取得、ターゲット広告およびデータ販売のオプトアウトを行う権限を居住者に付与します。 この法律は、テキサス州で事業を行う事業体、またはテキサス州の居住者が消費する製品/サービスを製造する事業体に適用されます。中小企業やその他の特定の組織を除きます。 違反行為には民事罰が科されることがある。 |
| TIPA （テネシー州） | `tipa_tn_usa` | [[!DNL Tennessee Information Protection Act (TIPA)]](https://www.capitol.tn.gov/Bills/113/Bill/HB1181.pdf) はテネシー州の住民に対し、自身の個人情報にアクセスし、訂正し、削除し、その写しを入手する権利を認めている。 また、個人情報の販売、ターゲット広告、および特定の種類のプロファイリングをオプトアウトする権利も提供します。 この法律は、データ処理と収益に関する特定のしきい値を満たす企業に適用され、明確なプライバシー通知、データの最小化、合理的なセキュリティ対策が必要です。 執行権限は、テネシー州の司法長官に付与される。 |
| UCPA （ユタ州） | `ucpa_ut_usa` | [[!DNL Utah Consumer Privacy Act]](https://le.utah.gov/~2022/bills/static/SB0227.html) は、企業が収集した個人データの内容、企業による個人データの使用方法、企業による個人データの販売の有無を消費者が把握できる権利を作成します。 消費者は、ビジネスに対して、自分の個人データの削除または販売の停止を要求できます。 |
| VCDPA （バージニア） | `vcdpa_va_usa` | [[!DNL Virginia Consumer Data Protection Act (VCDPA)]](https://lis.virginia.gov/cgi-bin/legp604.exe?212+sum+HB2307) は、個人データへのアクセス、削除、修正の権利を含む、新しいデータプライバシー権をバージニア州の居住者（「コンシューマー」）に提供します。 また、消費者は、個人データの販売のオプトアウト、個人データに基づくプロファイリングのオプトアウト、および個人広告目的の処理を行う権利を有します。 |

{style="table-layout:auto"}

Privacy Service API を使用してアクセスリクエストと削除リクエストを送信、トラッキング、管理する方法については、[ プライバシージョブエンドポイントガイド ](../api/privacy-jobs.md) を参照してください。 このガイドには、使い始めるのに役立つ例とフォーマットの詳細が含まれています。

## 次の手順

サポートされる規制について詳しくは、次のドキュメントを参照してください。

* [プライバシー規制 FAQ](./faq.md)
* [プライバシー規制の用語](./terminology.md)

Experience Cloud アプリケーションに保存されたデータに対するカスタマーアクセスリクエストおよび削除リクエストのサポート方法については、[Privacy ServiceおよびExperience Cloud アプリケーション ](../experience-cloud-apps.md) のガイドを参照してください。
