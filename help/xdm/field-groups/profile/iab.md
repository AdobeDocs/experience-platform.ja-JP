---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；iab;tcf；同意；
solution: Experience Platform
title: IAB TCF 2.0 同意スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、XDM Individual Profile クラスの IAB TCF 2.0 同意スキーマフィールドグループの概要を説明します。
exl-id: 52a4fee8-d7f4-4f27-8e26-0c132985eb84
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 2%

---

# [!UICONTROL IAB TCF 2.0 Consentschema フィー] ルドグループ

>[!NOTE]
>
>このドキュメントでは、XDM Individual Profile クラスの [!UICONTROL IAB TCF 2.0 Consent] スキーマフィールドグループについて説明します。 XDM ExperienceEvent クラスを対象とするフィールドグループについては、代わりに次の [document](../event/iab.md) を参照してください。

[!UICONTROL IAB TCF 2.0 タイムスタ] ンプ付きのシリーズ IAB の同意文字列を取得するために使用されるクラスの標準スキーマフィールドグループを組み合わせ [[!DNL XDM Individual Profile] ](../../classes/individual-profile.md) て、同意変更パターンを経時的に追跡します。

![](../../images/field-groups/iab-profile.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `identityPrivacyInfo` | マップ | 顧客の個々の ID 値を異なる TCF コンセントストリングに関連付けるマップタイプオブジェクト。 このオブジェクトの構造の例を以下に示します。 |

{style=&quot;table-layout:auto&quot;}

次の JSON は、`identityPrivacyInfo` マップの構造を示しています。

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

例に示すように、 `xdm:identityPrivacyInfo` の各ルートレベルキーは、ID サービスが認識する ID 名前空間に対応しています。 次に、各名前空間プロパティには、キーがその名前空間の顧客の対応する ID 値と一致する 1 つ以上のサブプロパティが必要です。 この例では、顧客は、Experience CloudID(`ECID`) 値 `13782522493631189` で識別されます。

>[!NOTE]
>
>上記の例では、お客様の ID を表す単一の名前空間と値のペアを使用していますが、他の名前空間のキーを追加でき、各名前空間には複数の ID 値を持つことができ、それぞれに独自の TCF 同意設定を持ちます。

ID 値ごとに、 `identityIABConsent` プロパティを指定する必要があります。このプロパティは、ID の TCF 同意値を提供します。 このプロパティの値は、[[!UICONTROL Consent String] データ型 ](../../data-types/consent-string.md) に準拠している必要があります。

このフィールドグループの使用例について詳しくは、Platform](../../../landing/governance-privacy-security/consent/iab/overview.md) での [IAB TCF 2.0 のサポートに関するガイドを参照してください。 フィールドグループ自体について詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-privacy.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-privacy.schema.json)
