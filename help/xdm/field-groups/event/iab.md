---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；iab;tcf；同意；
solution: Experience Platform
title: IAB TCF 2.0 同意スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、XDM ExperienceEvent クラスの IAB TCF 2.0 同意スキーマフィールドグループの概要を説明します。
exl-id: c236d0d4-27bd-45d7-a912-d0e93a609254
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 2%

---

# [!UICONTROL IAB TCF 2.0 Consentschema フィー] ルドグループ

>[!IMPORTANT]
>
>このドキュメントでは、XDM ExperienceEvent クラスの [!UICONTROL IAB TCF 2.0 Consent] スキーマフィールドグループについて説明します。 このフィールドグループは、同意の変更イベントを経時的に追跡する場合にのみ使用してください。
>
>イベントデータに記録される同意の値は、自動強制ワークフローでは使用されないことに注意してください。 自動強制を実行するには、同意の値を XDM Individual Profile クラスに取り込み、リアルタイム顧客プロファイルに対して有効にする必要があります。
>
>XDM Individual Profile クラスを対象とするフィールドグループについては、代わりに次の [document](../profile/iab.md) を参照してください。

[!UICONTROL IAB TCF 2.0 タイムスタ] ンプ付きのシリーズ IAB の同意文字列を取得するために使用されるクラスの標準スキーマフィールドグループを組み合わせ [[!DNL XDM ExperienceEvent] ](../../classes/experienceevent.md) て、同意変更パターンを経時的に追跡します。

![](../../images/field-groups/iab-event.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `consentStrings` | [ 同意文字列 ](../../data-types/consent-string.md) の配列 | イベントに関連付けられたコンセントストリング値の配列。 |

{style=&quot;table-layout:auto&quot;}

このフィールドグループの使用例について詳しくは、Platform](../../../landing/governance-privacy-security/consent/iab/overview.md) での [IAB TCF 2.0 のサポートに関するガイドを参照してください。 フィールドグループ自体について詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-privacy.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-privacy.schema.json)
