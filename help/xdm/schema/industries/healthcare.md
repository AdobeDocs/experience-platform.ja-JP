---
title: ヘルスケア業界データモデル ERD
description: ヘルスケア業界向けの標準化されたデータモデルを記述したエンティティ関係図（ERD）を表示します。 このデータモデルは、Adobe Experience Platformで使用するエクスペリエンスデータモデル（XDM）と互換性があります。
exl-id: ebcf97ec-f5a4-46e5-b1ad-c80d55aa2c6e
source-git-commit: 2fd35c4ac29f43391f9dc03c636d20558b701be7
workflow-type: tm+mt
source-wordcount: '617'
ht-degree: 8%

---

# [!UICONTROL  ヘルスケア ] 業界データモデル ERD

次の ERD （エンティティ関係図）は、医療業界向けの標準化されたデータモデルを表しています。 ERD は、Adobe Experience Platformでのデータの保存方法を考慮して、正規化されていない方法で意図的に表示されます。

>[!NOTE]
>
>説明している ERD は、この業界のユースケースに合わせてデータをモデル化する方法の推奨事項です。 Platform でこのデータモデルを使用するには、推奨されるスキーマとその関係を自分で構築する必要があります。 詳しくは、UI での [ スキーマ ](../../ui/resources/schemas.md) および [ 関係 ](../../tutorials/relationship-ui.md) の管理に関するガイドを参照してください。

次の凡例を使用して、この ERD を解釈します。

* に示されている各エンティティは、基になる [ エクスペリエンスデータモデル（XDM）クラス ](../composition.md#class) に基づいています。
* 特定のエンティティについて、**太字** でマークされた各行はフィールドグループまたはデータタイプを表し、その行が提供する関連フィールドが太字なしのテキストで表示されます。
* 特定のエンティティで最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは「ID」としてマークされ、これらのプロパティの 1 つは「プライマリ ID」としてマークされます。
* エンティティ関係は、cookie ベースのイベントでは多くの場合、トランザクションを行った個人を特定できないので、非依存としてマークされます。

![ ヘルスケア業界データモデルのエンティティ関係図を示す画像 ](../../images/industries/healthcare.png)

>[!NOTE]
>
>各エンティティには、「_ID」フィールドが含まれます。このフィールドは、該当するレコードまたはイベントの一意の文字列識別子（`_id`）属性を表します。 このフィールドは、個々のレコードまたはイベントの一意性を追跡、データの重複を防ぎ、ダウンストリームのサービスでそのデータを検索するために使用されます。 場合によっては、`_id` は、[ユニバーサル固有識別子（UUID）](https://tools.ietf.org/html/rfc4122) または [グローバル固有識別子（GUID）](https://docs.microsoft.com/ja-jp/dotnet/api/system.guid?view=net-5.0)とすることができます。<br><br>**このフィールドは、個人に関連する ID を表すものではなく**、データ記録そのものを表していることを見極めることが重要です。人物、イベントまたはビジネスエンティティに関する ID データは、代わりに互換性のあるフィールドグループが提供する [ID フィールド ](../composition.md#identity) に降格させる必要があります。

## [!UICONTROL  ヘルスケア ] のユースケース

次の表に、いくつかの一般的なヘルスケアユースケースで推奨されるクラスとスキーマフィールドグループの概要を示します。

| ユースケース | 推奨されるクラスおよびフィールドグループ |
| --- | --- |
| 保険を求める消費者の間でデジタル獲得と体験を向上させます。 以下に例を示します。 <ul><li>ユーザーが一般的な情報（プラン、プラン名/プラン層、メディケイド、ウェルネスプログラムなど）を含むページにアクセスする場合、プロモーションメールを送信したり、広告が表示されるサードパーティプラットフォームでターゲットにしたりするために、ユーザーの行動と探しているものを理解します。</li><li>人々が心臓の健康とワクチン情報を探す際には、ワクチンに関連する心臓の健康に関する情報を送って、ブランド認知度を高めたり、ワクチンのスケジュールを設定してもらったりします。</li></ul> | <ul><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL  ヘルスケア会員の詳細 ]](../../field-groups/profile/healthcare-member-details.md)</li><li>[!UICONTROL  プラン ] クラスを使用する `planID` 属性とスキーマの間に確立された関係フィールド。</li></ul></li><li>**[[!UICONTROL 支払者]](../../classes/payer.md)**</li><li>**[[!UICONTROL 計画]](../../classes/plan.md)**:<ul><li>[[!UICONTROL  ヘルスケアプラン詳細 ]](../../field-groups/plan/healthcare-plan-details.md)</li></ul></li><li>**[[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL アプリケーションの詳細]](../../field-groups/event/application-details.md)</li><li>[[!UICONTROL  サイトツールの詳細 ]](../../field-groups/event/sitetool-details.md)</li><li>[[!UICONTROL  Campaign マーケティング詳細 ]](../../field-groups/event/campaign-marketing-details.md)</li></ul></li></ul> |
| 過去のオンライン行動と健康データに基づくターゲット広告を通じて、患者のデジタル獲得を強化します。 | <ul><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL  ヘルスケア会員の詳細 ]](../../field-groups/profile/healthcare-member-details.md)</li></ul></li><li>**[[!UICONTROL プロバイダー]](../../classes/provider.md)**:<ul><li>[[!UICONTROL  ヘルスケア提供組織 ]](../../field-groups/provider/healthcare-provider.md)</li></ul></li><li>**[[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL Web 詳細]](../../field-groups/event/web-details.md)</li><li>[[!UICONTROL Advertisingの詳細 ]](../../field-groups/event/advertising-details.md)</li></ul></li></ul> |
| 顧客が保険会社を見つけた方法を理解するために、異なるチャネルを通じて保険のマーケティングを追跡することで、医療プランへの登録とアカウント作成を改善します。 | <ul><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL  ヘルスケア会員の詳細 ]](../../field-groups/profile/healthcare-member-details.md)</li></ul></li><li>**[[!UICONTROL 支払者]](../../classes/payer.md)**</li><li>**[[!UICONTROL 計画]](../../classes/plan.md)**:<ul><li>[[!UICONTROL  ヘルスケアプラン詳細 ]](../../field-groups/plan/healthcare-plan-details.md)</li></ul></li><li>**[[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL Web 詳細]](../../field-groups/event/web-details.md)</li><li>[[!UICONTROL Advertisingの詳細 ]](../../field-groups/event/advertising-details.md)</li></ul></li></ul> |
| 医療保険の適用の失効を避けます。 | <ul><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL  ヘルスケア会員の詳細 ]](../../field-groups/profile/healthcare-member-details.md)</li></ul></li><li>**[[!UICONTROL 計画]](../../classes/plan.md)**:<ul><li>[[!UICONTROL  ヘルスケアプラン詳細 ]](../../field-groups/plan/healthcare-plan-details.md)</li></ul></li></ul> |
| ダイレクト・トゥ・カスタマー（DTC）広告を使用して、医薬品の情報をプロバイダーに提供する。 | <ul><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL  ヘルスケア会員の詳細 ]](../../field-groups/profile/healthcare-member-details.md)</li></ul></li><li>**[[!UICONTROL 医薬品]](../../classes/medication.md)**:<ul><li>[[!UICONTROL  ヘルスケア薬品 ]](../../field-groups/medication/healthcare-medication.md)</li></ul></li><li>**[[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL Web 詳細]](../../field-groups/event/web-details.md)</li><li>[[!UICONTROL Advertisingの詳細 ]](../../field-groups/event/advertising-details.md)</li></ul></li></ul> |
