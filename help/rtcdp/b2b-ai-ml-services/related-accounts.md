---
title: Real-Time CDP B2B Edition の関連するアカウント
type: Documentation
description: Experience PlatformReal-Time CDP B2B の関連アカウント機能の概要と詳細情報です。
exl-id: 37fd2cdb-87c0-4e5e-9599-ad4f397f7c28
source-git-commit: 14e3eff3ea2469023823a35ee1112568f5b5f4f7
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 25%

---

# Real-Time CDP B2B Edition の関連するアカウント

## 概要 {#overview}

B2B 企業では、多くの場合、顧客情報が複数のシステムに保存されており、それぞれのシステムには、同じ実世界のビジネスエンティティに関するデータの一部のみ、または矛盾するデータが含まれています。そのため、顧客を正確に把握することが難しく、B2B マーケティングや営業活動の効率や効果を低下させるという大きな課題を抱えています。

| ID | 名前 | Web サイト | 業界 | 都道府県 | Phone | 数量 > のオープン商談あり `$1 million` |
|---|---|---|---|---|---|---|
| 1 | Acme | acme.com | ソフトウェア | CA | (408)536-6000 |  |
| 2 | Acme | acm.com | ソフトウェア | CA | 4085366000 | x |
| 3 | Acme Inc |  |  | CA | (408)5366000 |  |
| 4 | Acme コンサルティングサービス | `http://www.acme.com/consulting` | テクノロジーコンサルティング | NY | (212)471-0904 | x |
| 5 | Acme IT |  |  | CA |  |  |

{style=&quot;table-layout:auto&quot;}

関連するアカウントを使用する場合 [!DNL Real-Time CDP B2B] では、参照しているアカウントに類似したアカウントのリストが表示されます。

![Experience PlatformUI の関連アカウントを示す画面。](/help/rtcdp/b2b-ai-ml-services/assets/related-accounts-in-ui.png)

この機能を使用すると、Experience PlatformUI でアカウントプロファイルの関連アカウントプロファイルを表示し、セグメント定義に関連アカウントを含めて、リーチを広げたり、セグメントにより幅広い条件を適用したりできます。

## 仕組み {#how-it-works}

毎日実行される機械学習ジョブでは、次の 3 つの要因に基づいて、類似したアカウントプロファイルをグループにクラスター化する階層アルゴリズムを使用します。

* 親アカウントのリンク
* Web ドメイン
* アカウント名

正常に処理されたジョブの後、アカウントプロファイルグループの各メンバーには、「関連アカウント」リストのタグが付けられます。 リストは、 **関連アカウント** 」タブをクリックし、セグメント定義で関連するアカウントを使用します。

詳しくは、ドキュメントを参照してください [プロファイルエンリッチメント関連のアカウントジョブ](/help/dataflows/ui/b2b/monitor-profile-enrichment.md).

## 関連アカウントの表示方法 {#how-to-view}

Experience PlatformUI で参照しているアカウントの関連アカウントを表示できます。

詳しくは、ドキュメントを参照してください [UI で関連アカウントを検索する方法](/help/rtcdp/accounts/account-profile-ui-guide.md#related-accounts-tab).

## 関連するアカウントの使用方法 {#how-to-use}

アカウントと関連するアカウントをセグメント化で使用できます。 セグメント定義で関連アカウントを使用するかどうかの決定は、マーケティングの使用例によって異なります。 例えば、電子メールマーケティングや広告キャンペーンに関連するアカウントを使用し、より広い範囲のリーチと引き換えに、より低い精度を受け入れることができます。

参照先 [セグメント化の例](/help/rtcdp/segmentation/b2b.md#related-accounts) 関連するアカウントを使用する
