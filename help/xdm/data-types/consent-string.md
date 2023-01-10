---
solution: Experience Platform
title: 同意文字列データタイプ
description: このドキュメントでは、同意文字列 XDM データタイプの概要を説明します。
exl-id: 288ec79e-074a-4d72-9c5f-e9cd8485b804
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 5%

---

# [!UICONTROL 同意文字列] データタイプ

[!UICONTROL 同意文字列] は、顧客の同意を表す string 値を表す標準的な XDM データ型です。 同意文字列の標準などのコンテキスト情報が含まれます ( 例えば、 [IAB Transparency and Consent Framework(TCF)2.0](../field-groups/profile/iab.md)) をクリックします。

![](../images/data-types/consent-string.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `consentStandard` | 文字列 | 同意文字列の標準。 これは、同意管理サービスが設定する同意文字列の形式を決定するのに役立ちます。 |
| `consentStandardVersion` | 文字列 | 同意管理サービスで設定される同意文字列の形式を正確に定義するために使用される、同意標準のバージョン。 |
| `consentStringValue` | 文字列 | 同意管理サービスによって提供される完全な同意文字列。 `consentStandard` および `consentStandardVersion` この文字列の解析方法を定義するのに役立ちます。 |
| `containsPersonalData` | ブール値 | このフィールドが true の場合、同意の実施のために、この同意文字列を処理する必要があることを意味します。 |
| `gdprApplies` | ブール値 | このフィールドが true の場合、個人データに同意が得られることを意味します。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/consent/consentstring.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/consent/consentstring.schema.json)
