---
title: 医療業界のデータモデル ERD
description: 医療業界の標準化されたデータモデルを示す ERD（エンティティ関係図）を表示します。 このデータモデルは、Adobe Experience Platformで使用する Experience Data Model(XDM) と互換性があります。
exl-id: ebcf97ec-f5a4-46e5-b1ad-c80d55aa2c6e
source-git-commit: 2fd35c4ac29f43391f9dc03c636d20558b701be7
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 21%

---

# [!UICONTROL 医療] 業界データモデル ERD

次の ERD（エンティティ関係図）は、医療業界向けの標準化されたデータモデルを表しています。 ERD は、Adobe Experience Platformでのデータの格納方法を考慮し、意図的に非正規化された方法で提示されています。

>[!NOTE]
>
>説明に従った ERD は、この業界の使用例でデータをモデル化する方法を推奨します。 Platform でこのデータモデルを使用するには、推奨スキーマとその関係を自分で構築する必要があります。 管理に関するガイドを参照してください。 [スキーマ](../../ui/resources/schemas.md) および [関係](../../tutorials/relationship-ui.md) （ UI 内）を参照してください。

この ERD を解釈するには、次の凡例を使用します。

* に表示される各エンティティは、基になる [エクスペリエンスデータモデル (XDM) クラス](../composition.md#class).
* 特定のエンティティに対し、 **太字** は、フィールドグループまたはデータ型を表し、次に示す関連フィールドを太字化されていないテキストで示します。
* 特定のエンティティの最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは「ID」としてマークされ、これらのプロパティの 1 つが「プライマリ ID」としてマークされます。
* Cookie ベースのイベントはトランザクションをおこなった個人や個人を特定できないことが多いので、エンティティの関係は非依存としてマークされます。

![医療業界のデータモデルのエンティティ関係図を示す画像](../../images/industries/healthcare.png)

>[!NOTE]
>
>各エンティティには、一意の文字列識別子 (`_id`) 属性を設定します。 このフィールドは、個々のレコードまたはイベントの一意性を追跡し、データの重複を防ぎ、ダウンストリームサービスでそのデータを検索するために使用します。 場合によっては、`_id` は、[ユニバーサル固有識別子（UUID）](https://tools.ietf.org/html/rfc4122) または [グローバル固有識別子（GUID）](https://docs.microsoft.com/ja-jp/dotnet/api/system.guid?view=net-5.0)とすることができます。<br><br>**このフィールドは、個人に関連する ID を表すものではなく**、データ記録そのものを表していることを見極めることが重要です。人物、イベントまたはビジネスエンティティに関する ID データは、に任せる必要があります [id フィールド](../composition.md#identity) 代わりに、互換性のあるフィールドグループによって提供されます。

## [!UICONTROL 医療] 使用例

次の表に、一般的な医療関連の使用例で推奨されるクラスとスキーマフィールドグループを示します。

| ユースケース | 推奨クラスとフィールドグループ |
| --- | --- |
| 保険の消費者の買い物の間で、デジタルの獲得と経験を改善します。 以下に例を示します。 <ul><li>ユーザーが一般情報（プラン、プラン名/層、医療、ウェルネスプログラムなど）を含むページにアクセスすると、プロモーション E メールを送信したり、広告を含むサードパーティプラットフォームでターゲット設定するために、ユーザーの行動と目的を理解できます。</li><li>人々が心臓の健康とワクチンの情報を検索する際に、心臓の健康に関するワクチン関連の情報を送信し、ブランド認知度を高めたり、ワクチンのスケジュールを設定するように求めます。</li></ul> | <ul><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL ヘルスケア会員の詳細]](../../field-groups/profile/healthcare-member-details.md)</li><li>A と B の間に確立された関係フィールド `planID` 属性とスキーマ [!UICONTROL プラン] クラス。</li></ul></li><li>**[[!UICONTROL 支払者]](../../classes/payer.md)**</li><li>**[[!UICONTROL プラン]](../../classes/plan.md)**:<ul><li>[[!UICONTROL ヘルスケアプランの詳細]](../../field-groups/plan/healthcare-plan-details.md)</li></ul></li><li>**[[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL アプリケーションの詳細]](../../field-groups/event/application-details.md)</li><li>[[!UICONTROL サイトツールの詳細]](../../field-groups/event/sitetool-details.md)</li><li>[[!UICONTROL  キャンペーンマーケティング詳細]](../../field-groups/event/campaign-marketing-details.md)</li></ul></li></ul> |
| 過去のオンライン行動と健康データに基づいて、ターゲット広告を通じて患者のデジタル獲得を増やします。 | <ul><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL ヘルスケア会員の詳細]](../../field-groups/profile/healthcare-member-details.md)</li></ul></li><li>**[[!UICONTROL プロバイダー]](../../classes/provider.md)**:<ul><li>[[!UICONTROL ヘルスケア提供組織]](../../field-groups/provider/healthcare-provider.md)</li></ul></li><li>**[[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL Web 詳細]](../../field-groups/event/web-details.md)</li><li>[[!UICONTROL 広告の詳細]](../../field-groups/event/advertising-details.md)</li></ul></li></ul> |
| 顧客が保険会社をどのように見つけたかを理解するために、様々なチャネルを通じて保険のマーケティングを追跡することで、医療プランの登録とアカウント作成を改善します。 | <ul><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL ヘルスケア会員の詳細]](../../field-groups/profile/healthcare-member-details.md)</li></ul></li><li>**[[!UICONTROL 支払者]](../../classes/payer.md)**</li><li>**[[!UICONTROL プラン]](../../classes/plan.md)**:<ul><li>[[!UICONTROL ヘルスケアプランの詳細]](../../field-groups/plan/healthcare-plan-details.md)</li></ul></li><li>**[[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL Web 詳細]](../../field-groups/event/web-details.md)</li><li>[[!UICONTROL 広告の詳細]](../../field-groups/event/advertising-details.md)</li></ul></li></ul> |
| 医療保険の損失を避ける。 | <ul><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL ヘルスケア会員の詳細]](../../field-groups/profile/healthcare-member-details.md)</li></ul></li><li>**[[!UICONTROL プラン]](../../classes/plan.md)**:<ul><li>[[!UICONTROL ヘルスケアプランの詳細]](../../field-groups/plan/healthcare-plan-details.md)</li></ul></li></ul> |
| DTC(Direct-to-Customer) 広告を使用して、薬の情報をプロバイダーにプロモートします。 | <ul><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL ヘルスケア会員の詳細]](../../field-groups/profile/healthcare-member-details.md)</li></ul></li><li>**[[!UICONTROL 医薬品]](../../classes/medication.md)**:<ul><li>[[!UICONTROL ヘルスケア薬品]](../../field-groups/medication/healthcare-medication.md)</li></ul></li><li>**[[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL Web 詳細]](../../field-groups/event/web-details.md)</li><li>[[!UICONTROL 広告の詳細]](../../field-groups/event/advertising-details.md)</li></ul></li></ul> |
