---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；個人プロファイル；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；iab;tcf；同意；
solution: Experience Platform
title: プロファイルスキーマの IAB TCF 2.0 同意フィールドグループ
description: XDM Individual Profile クラスの IAB TCF 2.0 同意スキーマフィールドグループについて説明します。
exl-id: 52a4fee8-d7f4-4f27-8e26-0c132985eb84
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 1%

---

# プロファイルスキーマの [!UICONTROL IAB TCF 2.0 同意 ] フィールドグループ

>[!NOTE]
>
>このドキュメントでは、XDM 個人プロファイルクラスの [!UICONTROL IAB TCF 2.0 同意 ] スキーマフィールドグループについて説明します。 XDM ExperienceEvent クラスを対象としたフィールドグループについては、代わりに次の [ ドキュメント ](../event/iab.md) を参照してください。

[!UICONTROL IAB TCF 2.0 Consent] は、同意の変更パターンを経時的に追跡するために、タイムスタンプ付きのシリーズ IAB 同意文字列をキャプチャするために使用される [[!DNL XDM Individual Profile]  クラス ](../../classes/individual-profile.md) の標準スキーマフィールドグループです。

![](../../images/field-groups/iab-profile.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `identityPrivacyInfo` | マップ | 顧客の個々の ID 値を異なる TCF 同意文字列に関連付けるマップタイプオブジェクト。 このオブジェクトの構造の例を以下に示します。 |

{style="table-layout:auto"}

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

この例が示すように、`xdm:identityPrivacyInfo` の各ルートレベルキーは、ID サービスによって認識される ID 名前空間に対応します。 次に、各名前空間プロパティには、その名前空間に対応する顧客の ID 値に一致するキーを持つサブプロパティが少なくとも 1 つ必要です。 この例では、Experience CloudID （`ECID`）の値が `13782522493631189` である顧客が識別されます。

>[!NOTE]
>
>上記の例では、単一の名前空間と値のペアを使用して顧客の ID を表していますが、他の名前空間のキーを追加して、各名前空間に複数の ID 値を設定でき、それぞれに独自の TCF 同意環境設定のセットを持たせることができます。

各 ID 値に対して、ID の TCF 同意値を提供する `identityIABConsent` プロパティを指定する必要があります。 このプロパティの値は、[[!UICONTROL  同意文字列 ] データタイプ ](../../data-types/consent-string.md) に準拠している必要があります。

このフィールドグループのユースケースについては、[Platform での IAB TCF 2.0 のサポート ](../../../landing/governance-privacy-security/consent/iab/overview.md) に関するガイドを参照してください。 フィールドグループ自体の詳細については、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-privacy.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-privacy.schema.json)
