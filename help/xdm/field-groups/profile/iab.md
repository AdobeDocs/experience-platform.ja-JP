---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；iab;tcf；同意；
solution: Experience Platform
title: IAB TCF 2.0同意スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、XDM Individual ProfileクラスのIAB TCF 2.0同意スキーマフィールドグループの概要を説明します。
source-git-commit: 9b75a69cc6e31ea0ad77048a6ec1541df2026f27
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 2%

---


# [!UICONTROL IAB TCF 2.0 Consentschemaフィー] ルドグループ

>[!NOTE]
>
>このドキュメントでは、XDM Individual Profileクラスの[!UICONTROL IAB TCF 2.0 Consent]スキーマフィールドグループについて説明します。 XDM ExperienceEventクラスを対象としたフィールドグループについては、代わりに、次の[document](../event/iab.md)を参照してください。

[!UICONTROL IAB TCF 2.0タイムスタン] プ付きのシリーズIABの同意文字列を取り込み、経時的に同意変更パターンを追跡するために使用されるクラスの標準スキーマフィールドグループを組み合わせま [[!DNL XDM Individual Profile] ](../../classes/individual-profile.md) す。

![](../../images/field-groups/iab-profile.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `identityPrivacyInfo` | マップ | 顧客の個々のID値を異なるTCFコンセントストリングに関連付けるマップタイプオブジェクト。 このオブジェクトの構造の例を以下に示します。 |

{style=&quot;table-layout:auto&quot;}

次のJSONは、`identityPrivacyInfo`マップの構造を示しています。

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

例に示すように、 `xdm:identityPrivacyInfo`の各ルートレベルキーは、IDサービスで認識されるID名前空間に対応します。 次に、各名前空間プロパティには、その名前空間の顧客の対応するID値とキーが一致するサブプロパティが少なくとも1つ必要です。 この例では、顧客は、Experience CloudID(`ECID`)値`13782522493631189`で識別されます。

>[!NOTE]
>
>上の例では、顧客のIDを表すために1つの名前空間と値のペアを使用していますが、他の名前空間にキーを追加でき、各名前空間には複数のID値を持つことができ、それぞれにTCF同意設定のセットがあります。

ID値ごとに、 `identityIABConsent`プロパティを指定する必要があります。このプロパティは、IDのTCF同意値を提供します。 このプロパティの値は、[[!UICONTROL Consent String]データ型](../../data-types/consent-string.md)に準拠している必要があります。

このフィールドグループの使用例について詳しくは、Platform](../../../landing/governance-privacy-security/consent/iab/overview.md)での[IAB TCF 2.0のサポートに関するガイドを参照してください。 フィールドグループ自体について詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-privacy.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-privacy.schema.json)
