---
solution: Experience Platform
title: 同意文字列データタイプ
description: 同意文字列 XDM データタイプについて説明します。
exl-id: 288ec79e-074a-4d72-9c5f-e9cd8485b804
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 13%

---

# [!UICONTROL &#x200B; 同意文字列 &#x200B;] データタイプ

[!UICONTROL &#x200B; 同意文字列 &#x200B;] は、顧客の同意を表す文字列値を記述する標準 XDM データタイプです。 同意文字列の標準（[IAB Transparency and Consent Framework （TCF） 2.0](../field-groups/profile/iab.md) など）などのコンテキスト情報が含まれます。

![](../images/data-types/consent-string.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `consentStandard` | 文字列 | 同意文字列の標準。 これは、同意管理サービスが設定する同意文字列の形式を決定するのに役立ちます。 |
| `consentStandardVersion` | 文字列 | 同意管理サービスが設定する同意文字列の形式を正確に定義するために使用される、同意標準のバージョン。 |
| `consentStringValue` | 文字列 | 同意管理サービスが提供する完全な同意文字列。 `consentStandard` と `consentStandardVersion` は、この文字列の解析方法を定義するのに役立ちます。 |
| `containsPersonalData` | ブール値 | このフィールドが true の場合、同意の実施のために、この同意文字列が処理される必要があるということを意味します。 |
| `gdprApplies` | ブール値 | このフィールドが true の場合、個人データに同意が得られるということを意味します。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/consent/consentstring.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/consent/consentstring.schema.json)
