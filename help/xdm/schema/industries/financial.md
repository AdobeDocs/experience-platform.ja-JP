---
solution: Experience Platform
title: 金融サービス業界のデータモデルERD
topic-legacy: overview
description: 銀行業、金融サービス業、保険業(BFSI)業界の標準化されたデータモデルを示すエンティティ関係図(ERD)を表示します。 このデータモデルは、Adobe Experience Platformで使用するエクスペリエンスデータモデル(XDM)と互換性があります。
exl-id: 2e8f6b2a-10e7-4394-b45f-c03db0f25400
source-git-commit: 38fa2345cb87e50bd4c8788996f03939fb199cf9
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---

# [!UICONTROL 金融サ] ービス業界データモデルERD

次のエンティティ関係図(ERD)は、銀行、金融サービス、保険(BFSI)業界向けの標準化されたデータモデルを表しています。 ERDは、Adobe Experience Platformでのデータの格納方法を考慮して、意図的に非正規化方式で提示されます。

>[!NOTE]
>
>説明に従うERDは、この業界の使用例に合わせてデータをモデル化する方法を推奨します。 Platformでこのデータモデルを利用するには、推奨されるスキーマとその関係を自分で作成する必要があります。 詳しくは、UIでの[スキーマ](../../ui/resources/schemas.md)と[関係](../../tutorials/relationship-ui.md)の管理に関するガイドを参照してください。

次の凡例を使用して、このERDを解釈します。

* に表示される各エンティティは、基になる[エクスペリエンスデータモデル(XDM)クラス](../composition.md#class)に基づいています。
* 特定のエンティティに対して、**太字**&#x200B;でマークされた各行は、フィールドグループまたはデータ型を表し、次に示す関連フィールドが太字で示されています。
* 特定のエンティティの最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは、「ID」としてマークされ、これらのプロパティの1つが「プライマリID」としてマークされます。
* Cookieベースのイベントはトランザクションをおこなった個人や個人を特定できないことが多いので、エンティティの関係は非依存としてマークされます。

![](../../images/industries/financial.png)

>[!NOTE]
>
>エクスペリエンスイベントエンティティには、XDM ExperienceEventクラスで提供される一意の識別子(`_id`)属性を表す「_ID」フィールドが含まれます。 この値に何が求められるかについて詳しくは、[XDM ExperienceEvent](../../classes/experienceevent.md)の参照ドキュメントを参照してください。