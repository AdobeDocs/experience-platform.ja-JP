---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；iab;tcf；同意；
solution: Experience Platform
title: プロファイルスキーマ用の IAB TCF 2.0 同意フィールドグループ
description: このドキュメントでは、XDM Individual Profile クラスの IAB TCF 2.0 同意スキーマフィールドグループの概要を説明します。
exl-id: 52a4fee8-d7f4-4f27-8e26-0c132985eb84
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 1%

---

# [!UICONTROL IAB TCF 2.0 同意] プロファイルスキーマのフィールドグループ

>[!NOTE]
>
>このドキュメントでは、 [!UICONTROL IAB TCF 2.0 同意] XDM Individual Profile クラスのスキーマフィールドグループ。 XDM ExperienceEvent クラスを対象とするフィールドグループについては、次を参照してください。 [文書](../event/iab.md) 代わりに、

[!UICONTROL IAB TCF 2.0 同意] は、 [[!DNL XDM Individual Profile] クラス](../../classes/individual-profile.md) タイムスタンプ付きのシリーズ IAB の同意文字列を取り込んで、同意変更パターンを経時的に追跡するために使用します。

![](../../images/field-groups/iab-profile.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `identityPrivacyInfo` | マップ | 顧客の個々の ID 値を異なる TCF コンセントストリングに関連付けるマップタイプオブジェクト。 このオブジェクトの構造の例を以下に示します。 |

{style="table-layout:auto"}

次の JSON は、 `identityPrivacyInfo` マップ。

```json
{
  "identityPrivacyInfo": {
    "ECID": {
      "13782522493631189": {
        "identityIABConsent": {
          "consentTimestamp": "2020-04-11T05:05:05Z",
          "consentString": {
            "consentStandard": "IAB TCF",
            "consentStandardVersion": "2.0",
            "consentStringValue": "BObdrPUOevsguAfDqFENCNAAAAAmeAAA.PVAfDObdrA.DqFENCAmeAENCDA",
            "gdprApplies": true,
            "containsPersonalData": false
          }
        }
      }
    }
  }
}
```

例で示すように、の各ルートレベルキー `xdm:identityPrivacyInfo` は、ID サービスで認識される id 名前空間に対応します。 次に、各名前空間プロパティには、キーがその名前空間の顧客の対応する ID 値と一致する、少なくとも 1 つのサブプロパティが必要です。 この例では、顧客はExperience CloudID(`ECID`) の値 `13782522493631189`.

>[!NOTE]
>
>上の例では、1 つの名前空間と値のペアを使用して顧客の ID を表しますが、他の名前空間のキーを追加できます。また、各名前空間には複数の ID 値を持つことができ、それぞれに TCF 同意設定のセットがあります。

ID 値ごとに、 `identityIABConsent` プロパティを指定する必要があります。このプロパティは、ID の TCF 同意値を提供します。 このプロパティの値は、 [[!UICONTROL 同意文字列] データタイプ](../../data-types/consent-string.md).

詳しくは、 [Platform での IAB TCF 2.0 のサポート](../../../landing/governance-privacy-security/consent/iab/overview.md) を参照してください。 フィールドグループ自体について詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-privacy.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-privacy.schema.json)
