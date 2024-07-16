---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；iab;tcf；同意；
solution: Experience Platform
title: イベントスキーマの IAB TCF 2.0 同意フィールドグループ
description: XDM ExperienceEvent クラスの IAB TCF 2.0 同意スキーマフィールドグループについて説明します。
exl-id: c236d0d4-27bd-45d7-a912-d0e93a609254
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 1%

---

# イベントスキーマの [!UICONTROL IAB TCF 2.0 同意 ] フィールドグループ

>[!IMPORTANT]
>
>このドキュメントでは、XDM ExperienceEvent クラスの [!UICONTROL IAB TCF 2.0 同意 ] スキーマフィールドグループについて説明します。 このフィールドグループは、同意変更イベントを経時的に追跡する場合にのみ使用してください。
>
>イベントデータに記録された同意値は、自動適用ワークフローでは考慮されません。 自動適用を実行するには、同意値を XDM 個人プロファイル クラスに取り込み、リアルタイム顧客プロファイルに対して有効にする必要があります。
>
>XDM 個人プロファイルクラス向けのフィールドグループについては、代わりに次の [ ドキュメント ](../profile/iab.md) を参照してください。

[!UICONTROL IAB TCF 2.0 Consent] は、同意の変更パターンを経時的に追跡するために、タイムスタンプ付きのシリーズ IAB 同意文字列をキャプチャするために使用される [[!DNL XDM ExperienceEvent]  クラス ](../../classes/experienceevent.md) の標準スキーマフィールドグループです。

![](../../images/field-groups/iab-event.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `consentStrings` | [ 同意文字列 ](../../data-types/consent-string.md) の配列 | イベントに関連付けられた同意文字列値の配列。 |

{style="table-layout:auto"}

このフィールドグループのユースケースについては、[Platform での IAB TCF 2.0 のサポート ](../../../landing/governance-privacy-security/consent/iab/overview.md) に関するガイドを参照してください。 フィールドグループ自体の詳細については、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-privacy.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-privacy.schema.json)
