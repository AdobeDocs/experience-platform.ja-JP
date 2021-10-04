---
solution: Experience Platform
title: Consent String Data Type
topic-legacy: overview
description: このドキュメントでは、Consent String XDM データタイプの概要を説明します。
exl-id: 288ec79e-074a-4d72-9c5f-e9cd8485b804
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 5%

---

# [!UICONTROL 同意文字列] データタイプ

[!UICONTROL 同意文] 字列は、顧客の同意を表す文字列値を記述する標準の XDM データ型です。同意文字列の標準など、コンテキスト情報が含まれます ( 例えば、[IAB Transparency and Consent Framework(TCF)2.0](../field-groups/profile/iab.md))。

![](../images/data-types/consent-string.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `consentStandard` | 文字列 | コンセントストリングの標準。 これは、同意管理サービスで設定されるコンセントストリングの形式を判断するのに役立ちます。 |
| `consentStandardVersion` | 文字列 | 同意管理サービスで設定される同意文字列の形式を正確に定義するために使用される、同意標準のバージョン。 |
| `consentStringValue` | 文字列 | 同意管理サービスが提供する完全なコンセントストリング。 `consentStandard` および `consentStandardVersion` は、この文字列の解析方法の定義に役立ちます。 |
| `containsPersonalData` | Boolean | このフィールドが true の場合、同意の実施のためにこのコンセントストリングを処理する必要があります。 |
| `gdprApplies` | Boolean | このフィールドが true の場合、同意に個人データが含まれていることを意味します。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/consent/consentstring.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/consent/consentstring.schema.json)
