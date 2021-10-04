---
solution: Experience Platform
title: 通信業界のデータモデル ERD
topic-legacy: overview
description: Adobe Experience Platformで使用するエクスペリエンスデータモデル (XDM) と互換性のある、通信業界用の標準化されたデータモデルを示すエンティティ関係図 (ERD) を表示します。
exl-id: 96f267ce-a177-4384-a512-841c89d942ba
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

#  通信業界データモデル ERD

次のエンティティ関係図 (ERD) は、通信業界向けの標準化されたデータモデルを表しています。 ERD は、データのAdobe Experience Platformへの格納方法を考慮して、意図的に非正規化方式で提示されています。

>[!NOTE]
>
>説明に従った ERD は、この業界の使用例でデータをモデル化する方法を推奨します。 Platform でこのデータモデルを使用するには、推奨されるスキーマとその関係を自分で作成する必要があります。 詳しくは、UI での [ スキーマ ](../../ui/resources/schemas.md) と [ 関係 ](../../tutorials/relationship-ui.md) の管理に関するガイドを参照してください。

次の凡例を使用して、この ERD を解釈します。

* に示す各エンティティは、基礎となる [ エクスペリエンスデータモデル (XDM) クラス ](../composition.md#class) に基づいています。
* 特定のエンティティの各行が **bold** でマークされている場合、フィールドグループまたはデータ型を表し、次のフィールドが太字で示されています。
* 特定のエンティティの最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは「ID」としてマークされ、これらのプロパティの 1 つが「プライマリ ID」としてマークされます。
* Cookie ベースのイベントは、多くの場合、トランザクションをおこなった個人や個人を特定できないので、エンティティの関係は非依存としてマークされます。


![](../../images/industries/telecom.png)

>[!NOTE]
>
>エクスペリエンスイベントエンティティには、XDM ExperienceEvent クラスが提供する一意の識別子 (`_id`) 属性を表す「_ID」フィールドが含まれます。 この値に何が予想されるかについて詳しくは、[XDM ExperienceEvent](../../classes/experienceevent.md) の参照ドキュメントを参照してください。
