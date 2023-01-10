---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；iab;tcf；同意；
solution: Experience Platform
title: イベントスキーマ用の IAB TCF 2.0 同意フィールドグループ
description: このドキュメントでは、XDM ExperienceEvent クラスの IAB TCF 2.0 同意スキーマフィールドグループの概要を説明します。
exl-id: c236d0d4-27bd-45d7-a912-d0e93a609254
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 2%

---

# [!UICONTROL IAB TCF 2.0 同意] イベントスキーマのフィールドグループ

>[!IMPORTANT]
>
>このドキュメントでは、 [!UICONTROL IAB TCF 2.0 同意] XDM ExperienceEvent クラスのスキーマフィールドグループです。 このフィールドグループは、同意の変更イベントを経時的に追跡する場合にのみ使用してください。
>
>イベントデータに記録される同意の値は、自動強制ワークフローでは使用されないことに注意してください。 自動強制を実行するには、同意の値を XDM Individual Profile クラスに取り込み、リアルタイム顧客プロファイルに対して有効にする必要があります。
>
>XDM Individual Profile クラスを対象とするフィールドグループについては、次を参照してください。 [文書](../profile/iab.md) 代わりに、

[!UICONTROL IAB TCF 2.0 同意] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md) タイムスタンプ付きのシリーズ IAB の同意文字列を取り込んで、同意変更パターンを経時的に追跡するために使用します。

![](../../images/field-groups/iab-event.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `consentStrings` | の配列 [同意文字列](../../data-types/consent-string.md) | イベントに関連付けられた同意文字列値の配列。 |

{style=&quot;table-layout:auto&quot;}

詳しくは、 [Platform での IAB TCF 2.0 のサポート](../../../landing/governance-privacy-security/consent/iab/overview.md) を参照してください。 フィールドグループ自体について詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-privacy.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-privacy.schema.json)
