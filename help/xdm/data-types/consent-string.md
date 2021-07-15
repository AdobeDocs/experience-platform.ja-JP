---
solution: Experience Platform
title: Consent String Data Type
topic-legacy: overview
description: このドキュメントでは、コンセントストリングXDMデータタイプの概要を説明します。
source-git-commit: 7a0ac3970713e95438c6f0fdbd6175545ea7fdd0
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 5%

---

# [!UICONTROL 同意文字列] データタイプ

[!UICONTROL 同意文] 字列は、顧客の同意を表す文字列値を記述する標準のXDMデータ型です。同意文字列の標準などのコンテキスト情報が含まれます(例えば、[IAB Transparency and Consent Framework(TCF)2.0](../field-groups/profile/iab.md))。

![](../images/data-types/consent-string.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `consentStandard` | 文字列 | コンセントストリングの標準。 これは、同意管理サービスによって設定される同意文字列の形式を判断するのに役立ちます。 |
| `consentStandardVersion` | 文字列 | 同意管理サービスで設定された同意文字列の形式を正確に定義するために使用される、同意標準のバージョン。 |
| `consentStringValue` | 文字列 | 同意管理サービスによって提供される完全なコンセントストリング。 `consentStandard` とは、 `consentStandardVersion` この文字列の解析方法を定義するのに役立ちます。 |
| `containsPersonalData` | Boolean | このフィールドがtrueの場合、同意の実施のためにこのコンセントストリングを処理する必要があります。 |
| `gdprApplies` | Boolean | このフィールドがtrueの場合、同意は個人データを含むことを意味します。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/consent/consentstring.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/consent/consentstring.schema.json)
