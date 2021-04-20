---
keywords: Experience Platform；ホーム；人気の高いトピック；Marketo Engage；マーケティング担当；マーケティング担当
solution: Experience Platform
title: Marketo Engageコネクタ
topic: 概要
description: このドキュメントでは、Marketo Engageソースコネクタの認証、マッピング、およびデータ遅延に関する情報を含む、認証ソースコネクタの概要を説明します。
translation-type: tm+mt
source-git-commit: 2563b413ec35cb4c5f05a54bce6f7271917e51f3
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 13%

---


# （ベータ版） [!DNL Marketo Engage]コネクタ

>[!IMPORTANT]
>
>[!DNL Marketo Engage]ソースは現在ベータ版です。 機能とドキュメントは変更される可能性があります。

Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[[!DNL Marketo Engage]](https://www.marketo.com/software/) (以下「[!DNL Marketo]」と呼ばれる)は、リードマネジメントとB2Bマーケター向けの完全なソリューションであり、複雑な購入ジャーニーのあらゆる段階を通じて顧客体験を変えることを検討しています。

[!DNL Marketo]ソースコネクタを使うと、B2Bデータを[!DNL Marketo]からプラットフォームに持ち込み、プラットフォームに接続されたアプリケーションを使って、このデータを最新の状態に保つことができます。

このドキュメントでは、コネクタの認証方法、[!DNL Marketo]フィールドをエクスペリエンスデータモデル(XDM)にマップする方法、コネクタのデータ待ち時間など、[!DNL Marketo]ソースコネクタの概要を説明します。

## [!DNL Marketo]コネクタを認証します

[!DNL Marketo]をプラットフォームに接続するには、まず`munchkinId`、`clientId`、および`clientSecret`の値を取得する必要があります。

秘密鍵証明書を取得するには、「[Marketoソースコネクタを認証する](./marketo-auth.md)」ドキュメントの手順を参照してください。

## エクスペリエンスデータモデル（XDM）

XDMは、サードパーティのソースからデータを取り込み、ダウンストリームプラットフォームサービスで使用できるようにする共通の構造と定義を提供する、公式に文書化された仕様です。

XDM標準に準拠することで、データをプラットフォームエコシステムに一貫して組み込むことができ、データの配信と情報の収集が容易になります。

XDMとそのプラットフォームでの役割について詳しくは、[XDM System overview](../../../../xdm/home.md)を参照してください。

## [!DNL Marketo]からXDMへのフィールドマッピング

[!DNL Marketo]とプラットフォームの間にソース接続を確立するには、Platformに取り込む前に、Marketoソースデータフィールドを適切なターゲットXDMフィールドにマップする必要があります。

[!DNL Marketo]データセットとプラットフォーム間のフィールドマッピング規則について詳しくは、次を参照してください。

* [アクティビティ](../mapping/marketo.md#activities)
* [プログラム](../mapping/marketo.md#programs)
* [プログラムメンバーシップ](../mapping/marketo.md#program-memberships)
* [会社](../mapping/marketo.md#companies)
* [静的リスト](../mapping/marketo.md#static-lists)
* [静的リストメンバーシップ](../mapping/marketo.md#static-list-memberships)
* [名前付きアカウント](../mapping/marketo.md#named-accounts)
* [オポチュニティ](../mapping/marketo.md#opportunities)
* [営業案件連絡先ロール](../mapping/marketo.md#opportunity-contact-roles)
* [人](../mapping/marketo.md#persons)

## プラットフォーム上の[!DNL Marketo]データの待ち時間が予想されます

次の表に、取り込みの性質と目的に応じて、[!DNL Marketo]データをプラットフォームに取り込むための待ち時間の概要を示します。

| 宛先 | 予想される遅延 |
| ----------- | ---------------- |
| [!DNL Real-time Customer Profile] | &lt; 1 分 |
| データレイク | &lt; 60 分 |

## 次の手順とその他のリソース

[!DNL Marketo]ソース接続の作成に関する詳細は、次のドキュメントを参照してください。

* [!DNL Marketo]データをプラットフォームに接続する方法については、UI](../../../tutorials/ui/create/adobe-applications/marketo.md)で[Marketoソースコネクタを作成する方法のチュートリアルを参照してください。
* [!DNL Marketo]で使用されるB2B名前空間とスキーマの基本的な設定については、[B2B名前空間とスキーマ](./marketo-namespaces.md)のドキュメントを参照してください。
* [!DNL Marketo] munchkin IDの検索と資格情報の生成については、[[!DNL Marketo] 認証ガイド](./marketo-auth.md)を参照してください。
* [!DNL Marketo]データセットに適用される特定のマッピングルールについて詳しくは、[[!DNL Marketo] field mappings](../mapping/marketo.md)のドキュメントを参照してください。