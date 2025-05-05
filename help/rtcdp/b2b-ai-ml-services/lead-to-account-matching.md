---
title: Real-Time CDP B2B でのリードとアカウントのマッチング
type: Documentation
description: 概要と、Experience Platform CDP B2B のリードとアカウントのマッチング機能の詳細を説明します。
feature: Get Started, Profiles, B2B
badgeB2B: label="B2B edition" type="Informative" url="https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html newtab=true"
exl-id: 2f853599-6bca-4ba6-bbba-131a49d8854e
source-git-commit: 4ba609e777716b1b38f5b143587e5476d851e344
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 3%

---

# Real-Time CDP B2B でのリードとアカウントのマッチング

## 概要 {#overview}

アカウントベースのマーケティングは、B2B マーケティングにとってますます重要な戦略となっています。 アカウントベースドマーケティングは、特定の高価値顧客を獲得する際に、次のような主なメリットを提供します。

- ROI のクリア
- 販売とマーケティングの連携
- パーソナライズされたアプローチ
- 無駄なリソースの削減
- 販売サイクルの短縮

アカウントベースのマーケティングは、既知の顧客をセールスアカウントにリンクする機能を提供します。 これにより、マーケティングチームは、カスタマージャーニーの早い段階でターゲットアカウントの見込み客とエンゲージし、コンバージョンの可能性を高めることができます。 既知の人物レコードには、通常、次の情報の一部または全部が含まれます。

- 氏名
- メールアドレス
- 連絡先番号
- 会社名
- 会社の web サイト
- 役職
- ロケーション

## 仕組み {#how-it-works}

毎日実行されるジョブでは、決定論的要因と確率論的要因の両方を使用して、既存のアカウントの関連付けなしで既知のリードプロファイルに一致します。 既知のリードプロファイルには、次の属性のいずれかが使用できます。

- b2b.companyName
- b2b.companyWebsite
- workEmail

>[!NOTE]
>
> b2b.personKey.sourceKey 属性が存在する必要があります。

b2b.companyName、b2b.companyWebsite および b2b.personKey.sourceKey 属性は、B2B person スキーマの b2b フィールドグループに配置できます。

属性を表示する ![B2B 人物スキーマ ](/help/rtcdp/accounts/images/b2b-person-schema.png)

workEmail 属性は、B2B person スキーマの最上位のフィールドグループとして見つけることができます。

workEmail を表示する ![B2B 人物スキーマ ](/help/rtcdp/accounts/images/b2b-person-workemail.png)

プロファイルは、一致スコアが内部の信頼性しきい値を超えた場合にのみ最適に一致します。 結果は、既存のアカウント人物関係 XDM の新しいシステムデータセットに保存されます。

リードとアカウントのマッチングサービスは、24 時間に 1 回の新しいユーザープロファイルのスナップショットが使用可能になったときに実行されます。 [ リードとアカウントのマッチングの設定 ](/help/rtcdp/accounts/account-profile-ui-guide.md) について詳しくは、ドキュメントを参照してください。

## リードとアカウントのマッチング出力の表示方法 {#how-to-view}

ジョブの実行後、結果は、既存のアカウント人物関係 XDM の新しいデータセットに保存されます。

データセットをプレビューするには、右上の **[!UICONTROL データセットをプレビュー]** を選択します。

![ 新しいデータセット ](/help/rtcdp/accounts/images/b2b-dataset-output.png)

データセットには、一致したアカウント情報と、選択したデータセットの一致スコアが含まれます。 「**[!UICONTROL 関係Source]**」フィールドは、リードとアカウントのマッチングプロセスによって決定されたかどうかを示します。

![ データセットの信頼性スコアと出力のプレビュー ](/help/rtcdp/accounts/images/b2b-dataset-preview.png)

## リードとアカウントのマッチングジョブの監視 {#monitoring-jobs}

ダッシュボードを使用して、リードとアカウントの一致するジョブのジョブステータスおよび関連指標を監視できます。

[ リードとアカウントのマッチングのジョブ監視 ](/help/dataflows/ui/b2b/monitor-profile-enrichment.md) について詳しくは、ドキュメントを参照してください。
