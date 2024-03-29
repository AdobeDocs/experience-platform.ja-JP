---
title: Real-Time CDP B2B でのアカウントマッチングのリード
type: Documentation
description: Experience PlatformCDP B2B のアカウントマッチング機能へのリードの概要と詳細。
feature: Get Started, Profiles, B2B
badgeB2B: label="B2B エディション" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html newtab=true"
exl-id: 2f853599-6bca-4ba6-bbba-131a49d8854e
source-git-commit: db57fa753a3980dca671d476521f9849147880f1
workflow-type: tm+mt
source-wordcount: '626'
ht-degree: 5%

---

# Real-Time CDP B2B でのアカウントマッチングのリード

## 概要 {#overview}

アカウントベースドマーケティングは、B2B マーケティングのための重要な戦略です。 アカウントベースドマーケティングには、特定の高価値顧客を獲得するための次の主要なメリットがあります。

- ROI をクリア
- セールスとマーケティングの連携
- パーソナライズされたアプローチ
- 無駄なリソースを削減
- 販売サイクルの短縮

アカウントベースドマーケティングでは、既知の人と匿名の Web 訪問者をセールスアカウントにリンクする機能を提供します。 これにより、マーケティングチームは、カスタマージャーニーの初期段階でターゲットアカウントの潜在的なリードと連携し、コンバージョンの可能性を高めることができます。 既知の人物レコードには、通常、次の情報の一部またはすべてが含まれます。

- 氏名
- メールアドレス
- 連絡先番号
- 会社名
- 会社の Web サイト
- 役職
- ロケーション

リードとアカウントのマッチングにより、既知のユーザープロファイルをアカウントプロファイルに結合できます。その後、アカウントやオポチュニティなどの B2B コンテキストでデータのセグメント化やターゲット設定をおこなうことができます。 ユーザープロファイルは、次の 3 つのカテゴリに分類できます。

- **アカウント担当者プロファイル：** 人物プロファイルは、データソースからの関係を通じて少なくとも 1 つのアカウントプロファイルに既に関連付けられています。 これは、少なくとも 1 つの連絡先フラグメントが存在することを意味します。

>[!NOTE]
>
> リードとアカウントの照合ジョブの実行時に、アカウント担当者プロファイルが一致しません。

- **既知の担当者プロファイル：** 人物プロファイルがどのアカウントプロファイルにも関連付けられておらず、次の人物プロファイル属性の少なくとも 1 つに値が割り当てられています。

   - メールアドレス
   - 会社名
   - 会社の Web サイト

- **匿名担当者プロファイル：** 人物プロファイルはどのアカウントプロファイルにも関連付けられておらず、次の人物プロファイル属性にも値がありません。

   - メールアドレス
   - 会社名
   - 会社の Web サイト

>[!NOTE]
>
> 人物プロファイルは、複数のアカウントプロファイルに関連付けられる場合があります。 ただし、リードとアカウントの照合プロセスは、最も一致する最も良いリードにのみ一致します。 より広範な一致が必要な場合は、リードを関連するアカウント機能と連携させます。

## 仕組み {#how-it-works}

毎日実行するジョブは、決定論的な要因と確率論的な要因の両方を使用して、既存のアカウントの関連付けがない既知のリードプロファイルを照合します。 既知のリードプロファイルには、次の属性の 1 つを使用できます。

- b2b.companyName
- b2b.companyWebsite
- workEmail

>[!NOTE]
>
> b2b.personKey.sourceKey 属性が存在する必要があります。

b2b.companyName、b2b.companyWebsite および b2b.personKey.sourceKey 属性は、B2B 人物スキーマの b2b フィールドグループにあります。

![属性を示す B2B 人物スキーマ](/help/rtcdp/accounts/images/b2b-person-schema.png)

workEmail 属性は、B2B ユーザースキーマの最上位フィールドグループとして見つかります。

![workEmail を示す B2B 人物スキーマ](/help/rtcdp/accounts/images/b2b-person-workemail.png)

一致スコアが内部信頼性しきい値を超える場合にのみ、プロファイルは最適に一致します。 結果は、既存のアカウント人物関係 XDM の新しいシステムデータセットに保存されます。

リードトゥアカウントマッチングサービスは、新しい担当者プロファイルのスナップショットが使用可能になり、24 時間に 1 回実行されます。 詳しくは、ドキュメントを参照してください [リードとアカウントの照合の設定](/help/rtcdp/accounts/account-profile-ui-guide.md).

## リードとアカウントの照合出力の表示方法 {#how-to-view}

ジョブの実行後、結果は既存のアカウント人物関係 XDM の新しいデータセットに保存されます。

データセットをプレビューするには、「 **[!UICONTROL データセットをプレビュー]** を右上に表示します。

![新しいデータセット](/help/rtcdp/accounts/images/b2b-dataset-output.png)

データセットには、一致したアカウント情報と、選択したデータセットの一致スコアが含まれます。 The **[!UICONTROL 関係ソース]** 「 」フィールドは、リードからアカウント照合プロセスに送信されたかどうかを示します。

![データセットの信頼性スコアと出力のプレビュー](/help/rtcdp/accounts/images/b2b-dataset-preview.png)

## リードとアカウントの照合ジョブの監視 {#monitoring-jobs}

ダッシュボードを使用して、リードとアカウントの一致ジョブに関するジョブのステータスと関連指標を監視できます。

詳しくは、ドキュメントを参照してください [リードのアカウント照合のジョブの監視](/help/dataflows/ui/b2b/monitor-profile-enrichment.md).
