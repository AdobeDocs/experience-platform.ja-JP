---
solution: Experience Platform
title: 小売業界データモデル
topic-legacy: overview
description: Adobe Experience Platformで使用するエクスペリエンスデータモデル(XDM)と互換性のある、小売業界用の標準化されたデータモデルを表示します。
exl-id: 40cbb243-668b-4280-815f-1f94a06b6b87
source-git-commit: 9c5a4e064af0b46ff30b41afef71ca2fd3503a82
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 6%

---

#  小売業界のデータモデル

次のエンティティ関係図(ERD)は、小売業界向けの標準化されたデータモデルを表しています。 ERDは、Adobe Experience Platformでのデータの格納方法を考慮して、意図的に非正規化方式で提示されます。

次の凡例を使用して、このERDを解釈します。

* に表示される各エンティティは、基になる[エクスペリエンスデータモデル(XDM)クラス](../composition.md#class)に基づいています。
* 特定のエンティティに対して、**太字**&#x200B;でマークされた各行は、フィールドグループまたはデータ型を表し、次に示す関連フィールドが太字で示されています。
* 特定のエンティティの最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは、「ID」としてマークされ、これらのプロパティの1つが「プライマリID」としてマークされます。
* Cookieベースのイベントはトランザクションをおこなった個人や個人を特定できないことが多いので、エンティティの関係は非依存としてマークされます。

![](../../images/industries/retail.png)

>[!NOTE]
>
>エクスペリエンスイベントエンティティには、XDM ExperienceEventクラスで提供される一意の識別子(`_id`)属性を表す「_ID」フィールドが含まれます。 この値に何が求められるかについて詳しくは、[XDM ExperienceEvent](../../classes/experienceevent.md)の参照ドキュメントを参照してください。

##  小売の使用例

次の表に、一般的な小売の使用例で推奨されるクラスとスキーマフィールドグループの概要を示します。

| 使用例 | 推奨クラスとフィールドグループ |
| --- | --- |
| オンラインとオフラインのデータソースを組み合わせ、クロスデバイスIDとオンライン/オフラインIDを解決して、クロスチャネルおよびクロスデバイスの全体的なアトリビューションレポートを提供します。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[コマースの詳細](../../field-groups/event/commerce-details.md)</li><li>[Webの詳細](../../field-groups/event/web-details.md)</li></ul></li><li>**Product** (custom class)\*:<ul><li>製品（カスタムフィールドグループ）\*</li></ul></li></ul> |
| 様々なセグメントに対してターゲットを絞り、パーソナライズされたエクスペリエンスを提供して、収益を増やし、オムニチャネルオーケストレーションのプラットフォームを強化します。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[キャンペーンマーケティングの詳細](../../field-groups/event/campaign-marketing-details.md)</li><li>[チャネルの詳細](../../field-groups/event/channel-details.md)</li><li>[コマースの詳細](../../field-groups/event/commerce-details.md)</li><li>[環境の詳細](../../field-groups/event/environment-details.md)</li><li>[Webの詳細](../../field-groups/event/web-details.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[人口統計の詳細](../../field-groups/profile/demographic-details.md)</li><li>[個人の連絡先の詳細](../../field-groups/profile/personal-contact-details.md)</li><li>[勤務先の詳細](../../field-groups/profile/work-contact-details.md)</li></ul></li></ul> |
| マルチタッチ属性を分析し、マーケティングの効率を高めます。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[キャンペーンマーケティングの詳細](../../field-groups/event/campaign-marketing-details.md)</li><li>[チャネルの詳細](../../field-groups/event/channel-details.md)</li><li>[コマースの詳細](../../field-groups/event/commerce-details.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[人口統計の詳細](../../field-groups/profile/demographic-details.md)</li></ul></li></ul> |
| 男性と女性のセグメント化を改善し、電子メールの関連性を高めます。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[コマースの詳細](../../field-groups/event/commerce-details.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[人口統計の詳細](../../field-groups/profile/demographic-details.md)</li></ul></li><li>**Product** (custom class)\*:<ul><li>製品（カスタムフィールドグループ）\*</li></ul></li></ul> |
| ロイヤリティ（パートナー）データを取り込み、Web、電子メール、デジタルマーケティングの各チャネルにわたって関連製品情報を増やします。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[Webの詳細](../../field-groups/event/web-details.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[人口統計の詳細](../../field-groups/profile/demographic-details.md)</li><li>[ロイヤルティの詳細](../../field-groups/profile/loyalty-details.md)</li></ul></li><li>**Product** (custom class)\*:<ul><li>製品（カスタムフィールドグループ）\*</li></ul></li></ul> |
| 自動化されたパーソナライズされたEメールを通じて、買い物かごの放棄者を再ターゲット化する。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[コマースの詳細](../../field-groups/event/commerce-details.md)</li><li>[Webの詳細](../../field-groups/event/web-details.md)</li></ul></li><li>**Product** (custom class)\*:<ul><li>製品（カスタムフィールドグループ）\*</li></ul></li></ul> |

{style=&quot;table-layout:auto&quot;}

*\*将来のリリースでは標準製品クラスが予定されていますが、製品スキーマは現在、カスタムクラスを使用して構築されている必要があります。そのため、スキーマのクラスの構造と、スキーマに追加するフィールドグループの構造を手動で作成する必要があります。 詳しくは、XDM UIガイドの[カスタムクラス](../../ui/resources/classes.md#create)の作成に関する節を参照してください。*
