---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM;ExperienceEvent；フィールド；スキーマ;スキーマ;スキーマデザイン；mixin;mixin;enduserids;end-user;ids;
solution: Experience Platform
title: エンドユーザーIDの詳細Mixin
topic: overview
description: このドキュメントでは、エンドユーザーIDの詳細ミックスインの概要を説明します。
translation-type: tm+mt
source-git-commit: f9d8021643e72e3fbb5315b54a19815dcdaaa702
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 6%

---


# [!UICONTROL エンドユーザーID] 詳細Mixin

>[!NOTE]
>
>いくつかのミックスインの名前が変更されました。 詳しくは、[mixin name updates](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL エンドユーザーID] の詳細： [[!DNL XDM ExperienceEvent] クラスの標準ミックスインです](../../classes/individual-profile.md)。複数のAdobeアプリケーションにわたる個人のID情報の説明に使用されます。ミックスインはルートレベルの`endUserIDs`オブジェクトを提供します。このオブジェクト自体には読み取り専用の`_experience`フィールドが含まれ、その値はデータの取り込み時に自動的に更新されます。

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
