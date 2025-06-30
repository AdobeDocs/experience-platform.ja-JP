---
title: Adobe Experience Platform リリースノート 2022年11月
description: Adobe Experience Platform の 2022年11月 のリリースノート。
exl-id: 1048cfae-6e7a-4d05-a004-c5c095a17fc4
source-git-commit: 2e41a1716e057cd33e4635c11ba9c3cfc185418a
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 91%

---

# Adobe Experience Platform リリースノート

**リリース日：2022年11月23日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [データ収集](#data-collection)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [ソース](#sources)

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| イベント転送用の [!DNL AWS] 拡張機能 | [イベント転送](../../tags/ui/event-forwarding/overview.md)拡張機能を使用して、[!DNL Amazon Web Services]（[!DNL AWS]）にデータを送信できるようになりました。詳しくは、[[!DNL AWS] 拡張機能の概要](../../tags/extensions/server/aws/overview.md)を参照してください。 |
|  イベント転送用の [!DNL Google Ads Enhanced Conversions] 拡張機能 | [イベント転送](../../tags/ui/event-forwarding/overview.md)拡張機能を使用して、コンバージョンデータを [!DNL Google Ads] に送信できるようになりました。詳しくは、[[!DNL Google Ads Enhanced Conversions] 拡張機能の概要](../../tags/extensions/server/google-ads-enhanced-conversions/overview.md)を参照してください。 |
|  イベント転送用の [!DNL Microsoft Azure] 拡張機能 | [イベント転送](../../tags/ui/event-forwarding/overview.md)拡張機能を使用して、[!DNL Microsoft Azure] にデータを送信できるようになりました。詳しくは、[[!DNL Microsoft Azure] 拡張機能の概要](../../tags/extensions/server/azure/overview.md)を参照してください。 |

Experience Platformのデータ収集機能について詳しくは、[ データ収集の概要 ](../../collection/home.md) を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| スキーマに直接追加する際にカスタムクラスにフィールドを割り当てる | [個々のフィールドをスキーマに直接追加](../../xdm/ui/resources/schemas.md#add-individual-fields)する際に、以前はフィールドをその親リソースとしてフィールドグループに割り当てることしかできませんでした。現在は、フィールドグループに加えて、親リソースとして[カスタムクラスにフィールドを割り当てる](../../xdm/ui/resources/schemas.md#add-to-class)ことができます。 |

{style="table-layout:auto"}

Experience Platformの XDM について詳しくは、「[XDM システムの概要 ](../../xdm/home.md)」を参照してください。
