---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；iab;tcf；同意；
solution: Experience Platform
title: IAB TCF 2.0同意スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、XDM ExperienceEventクラスのIAB TCF 2.0同意スキーマフィールドグループの概要を説明します。
source-git-commit: 7a0ac3970713e95438c6f0fdbd6175545ea7fdd0
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 2%

---


# [!UICONTROL IAB TCF 2.0 Consentschemaフィー] ルドグループ

>[!IMPORTANT]
>
>このドキュメントでは、XDM ExperienceEventクラスの[!UICONTROL IAB TCF 2.0 Consent]スキーマフィールドグループについて説明します。 このフィールドグループは、同意の変更イベントを経時的に追跡する場合にのみ使用してください。
>
>イベントデータに記録される同意の値は、自動実施ワークフローでは考慮されないことに注意してください。 自動強制を実行するには、同意の値をXDM Individual Profileクラスに取り込み、リアルタイム顧客プロファイルを有効にする必要があります。
>
>XDM Individual Profileクラスを対象とするフィールドグループについては、代わりに、次の[document](../profile/iab.md)を参照してください。

[!UICONTROL IAB TCF 2.0タイムスタン] プ付きのシリーズIABの同意文字列を取り込み、経時的に同意変更パターンを追跡するために使用されるクラスの標準スキーマフィールドグループを組み合わせま [[!DNL XDM ExperienceEvent] ](../../classes/experienceevent.md) す。

![](../../images/field-groups/iab-event.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `consentStrings` | [同意文字列](../../data-types/consent-string.md)の配列 | イベントに関連付けられたコンセントストリング値の配列。 |

{style=&quot;table-layout:auto&quot;}

このフィールドグループの使用例について詳しくは、Platform](../../../landing/governance-privacy-security/consent/iab/overview.md)での[IAB TCF 2.0のサポートに関するガイドを参照してください。 フィールドグループ自体について詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-privacy.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-privacy.schema.json)
