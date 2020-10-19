---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;ExperienceEvent;fields;schemas;Schemas;Schema design;mixin;mixin;enduserids;end-user;end user;ids;
solution: Experience Platform
title: ExperienceEvent EndUserIDs mixin
topic: overview
description: このドキュメントでは、ExperienceEvent EndUserIDsミックスインの概要を説明します。
translation-type: tm+mt
source-git-commit: f5bddb39c16eb25e85297f56e331d3aa51510eb9
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 8%

---


# [!UICONTROL ExperienceEvent EndUserIDs] mixin

[!UICONTROL ExperienceEvent EndUserIDs] (EndUserIDs [[!DNL XDM ExperienceEvent] )は、](../../classes/individual-profile.md)クラスの標準ミックスインです。複数のAdobeアプリケーションで個人のID情報を記述するのに使用されます。 ミックスインは、ルートレベルの `endUserIDs` オブジェクトを提供します。ルートレベルのオブジェクト自体には読み取り専用の `_experience` フィールドが含まれ、データが取り込まれると自動的に値が更新されます。

<img src="../../images/mixins/enduserids.png" width="700" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `aacustomid` | [ID](../../data-types/identity.md) | Adobe Analytics CloudのカスタムエンドユーザーID。 |
| `aaid` | [ID](../../data-types/identity.md) | Adobe Analytics CloudのエンドユーザーID。 |
| `acid` | [ID](../../data-types/identity.md) | Adobe CampaignのエンドユーザーID。 |
| `adcloud` | [ID](../../data-types/identity.md) | Adobe Advertising CloudのエンドユーザーID。 |
| `emailid` | [ID](../../data-types/identity.md) | 電子メールアドレスID。 |
| `mcid` | [ID](../../data-types/identity.md) | Adobe Marketing CloudID。 |
| `phonenumberid` | [ID](../../data-types/identity.md) | 電話番号ID。 |
| `tntid` | [ID](../../data-types/identity.md) | Adobe TargetのエンドユーザーID。 |

Mixinの詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/mixins/experience-event/experienceevent-enduserids.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/mixins/experience-event/experienceevent-enduserids.schema.json)
