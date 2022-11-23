---
title: Adobe Experience Platformリリースノート 2022 年 11 月
description: Adobe Experience Platform の 2022年11月 のリリースノート。
source-git-commit: 3c78fc1cbfcd00f0fd5facfdf07bbc2757f492fa
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 78%

---

# Adobe Experience Platform リリースノート

**リリース日：2022 年 11 月 23 日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [データ収集](#data-collection)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [ソース](#sources)

## データ収集 {#data-collection}

Adobe Experience Platform は、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信します。そこでデータを強化、変換、アドビまたはアドビ以外の宛先への配信を可能にする一連のテクノロジーを提供します。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
|  イベント転送用の [!DNL AWS] 拡張機能 | これで、次にデータを送信できます： [!DNL Amazon Web Services] ([!DNL AWS]) を [イベント転送](../../tags/ui/event-forwarding/overview.md) 拡張子。 詳しくは、[[!DNL AWS] 拡張機能の概要](../../tags/extensions/web/aws/overview.md)を参照してください。 |
|  イベント転送用の [!DNL Google Ads Enhanced Conversions] 拡張機能 | これで、コンバージョンデータを [!DNL Google Ads] の使用 [イベント転送](../../tags/ui/event-forwarding/overview.md) 拡張子。 詳しくは、[[!DNL Google Ads Enhanced Conversions] 拡張機能の概要](../../tags/extensions/web/google-ads-enhanced-conversions/overview.md)を参照してください。 |
|  イベント転送用の [!DNL Microsoft Azure] 拡張機能 | [イベント転送](../../tags/ui/event-forwarding/overview.md)拡張機能を使用して、[!DNL Microsoft Azure] にデータを送信できるようになりました。詳しくは、[[!DNL Microsoft Azure] 拡張機能の概要](../../tags/extensions/web/azure/overview.md)を参照してください。 |

Platform のデータ収集機能について詳しくは、 [データ収集の概要](../../collection/home.md).

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| スキーマに直接追加する際にカスタムクラスにフィールドを割り当てる | 条件 [スキーマへの個々のフィールドの直接の追加](../../xdm/ui/resources/schemas.md#add-individual-fields)以前は、フィールドを親リソースとしてフィールドグループに割り当てることしかできませんでした。 現在は、フィールドグループに加えて、次の操作を実行できます。 [フィールドをカスタムクラスに割り当てる](../../xdm/ui/resources/schemas.md.md#add-to-class) を親リソースとして使用することをお勧めします。 |

{style=&quot;table-layout:auto&quot;}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- | 
| Oracle Service Cloud ソースのベータ版の可用性 | Oracle Service Cloud ソースを使用すると、Oracle Service Cloud アカウントから Experience Platform にデータを取り込むことができます。詳しくは、[Oracle Service Cloud ソース](../../sources/connectors/customer-success/oracle-service-cloud.md)に関するドキュメントを参照してください。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。