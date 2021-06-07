---
solution: Experience Platform
title: 旅行業界と接客業のデータモデルERD
topic-legacy: overview
description: Adobe Experience Platformで使用するエクスペリエンスデータモデル(XDM)と互換性のある、旅行業界や接客業向けの標準化されたデータモデルを示すエンティティ関係図(ERD)を表示します。
exl-id: 4d454160-9066-4702-815b-9509942f709e
source-git-commit: 88c17992a391b24a76c3e387d3033df4c75a6aa6
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---

# [!UICONTROL 旅行業界と] 病院業界のデータモデルERD

次のエンティティ関係図(ERD)は、旅行業界と接客業界向けの標準化されたデータモデルを表しています。 ERDは、Adobe Experience Platformでのデータの格納方法を考慮して、意図的に非正規化方式で提示されます。

次の凡例を使用して、このERDを解釈します。

* に表示される各エンティティは、基になる[エクスペリエンスデータモデル(XDM)クラス](../composition.md#class)に基づいています。
* 特定のエンティティに対して、**太字**&#x200B;でマークされた各行は、フィールドグループまたはデータ型を表し、次に示す関連フィールドが太字で示されています。
* 特定のエンティティの最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは、「ID」としてマークされ、これらのプロパティの1つが「プライマリID」としてマークされます。
* Cookieベースのイベントはトランザクションをおこなった個人や個人を特定できないことが多いので、エンティティの関係は非依存としてマークされます。

![](../../images/industries/travel-hospitality.png)

>[!NOTE]
>
>エクスペリエンスイベントエンティティには、XDM ExperienceEventクラスで提供される一意の識別子(`_id`)属性を表す「_ID」フィールドが含まれます。 この値に何が求められるかについて詳しくは、[XDM ExperienceEvent](../../classes/experienceevent.md)の参照ドキュメントを参照してください。