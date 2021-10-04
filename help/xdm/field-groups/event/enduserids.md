---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；enduserids；エンドユーザー；エンドユーザー；ids;
solution: Experience Platform
title: エンドユーザー ID 詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「エンドユーザー ID 詳細」スキーマフィールドグループの概要を説明します。
exl-id: ff5b74f4-7700-4d10-821e-b50f80ea8c05
source-git-commit: afe748d443aad7b6da5b348cd569c9e806e4419b
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 7%

---


# [!UICONTROL エンドユーザー ID 詳細スキ] ーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[ フィールドグループ名の更新 ](../name-updates.md) のドキュメントを参照してください。

[!UICONTROL エンドAdobeID ] クラスの標準スキーマフィールドグループを説明しま [[!DNL XDM ExperienceEvent] す](../../classes/experienceevent.md)。複数のユーザーアプリケーションにわたる個人の ID 情報を記述するのに使用されます。フィールドグループにはルートレベルの `endUserIDs` オブジェクトが用意されており、このオブジェクト自体には読み取り専用の `_experience` フィールドが含まれています。このフィールドの値はデータの取り込み時に自動的に更新されます。

<img src="../../images/field-groups/enduserids.png" width="700" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `aacustomid` | [ID](../../data-types/identity.md) | Adobe Analytics Cloudのカスタムエンドユーザー ID。 |
| `aaid` | [ID](../../data-types/identity.md) | Adobe Analytics Cloudのエンドユーザー ID。 |
| `acid` | [ID](../../data-types/identity.md) | Adobe Campaignのエンドユーザー ID。 |
| `adcloud` | [ID](../../data-types/identity.md) | Adobe Advertising Cloudのエンドユーザー ID。 |
| `emailid` | [ID](../../data-types/identity.md) | 電子メールアドレス ID。 |
| `mcid` | [ID](../../data-types/identity.md) | Adobe Marketing Cloud ID。 |
| `phonenumberid` | [ID](../../data-types/identity.md) | 電話番号 ID。 |
| `tntid` | [ID](../../data-types/identity.md) | Adobe Targetのエンドユーザー ID。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-enduserids.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-enduserids.schema.json)
