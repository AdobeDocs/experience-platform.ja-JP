---
keywords: Experience Platform、ホーム、人気のあるトピック、Marketo Engage、marketo engage、marketo
solution: Experience Platform
title: Marketo Engageコネクタ
topic-legacy: overview
description: このドキュメントでは、Marketo Engage、マッピング、データ遅延に関する情報など、認証ソースコネクタの概要を説明します。
exl-id: 063ec5d9-d643-4141-bf6d-878273f22b33
source-git-commit: 50e92ac8c1eccc9ccfb6b078ad8b996817a6d693
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 14%

---

# [!DNL Marketo Engage] コネクタ

Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[[!DNL Marketo Engage]](https://www.marketo.com/software/) ( 以下「[!DNL Marketo]」と呼ばれる ) は、複雑な購入ジャーニーの各ステージを通じてエンゲージメントをおこなうことで顧客体験を変えようとしているリード管理や B2B マーケター向けの完全なソリューションです。

[!DNL Marketo] ソースコネクタを使用すると、B2B データを [!DNL Marketo] から Platform に取り込み、Platform に接続されたアプリケーションを使用して、このデータを最新の状態に保つことができます。

このドキュメントでは、コネクタの認証方法、[!DNL Marketo] フィールドをエクスペリエンスデータモデル (XDM) にマッピングする方法、コネクタのデータ待ち時間など、[!DNL Marketo] ソースコネクタの概要を説明します。

## [!DNL Marketo] コネクタの認証

[!DNL Marketo] を Platform に接続するには、まず `munchkinId`、`clientId` および `clientSecret` の値を取得する必要があります。

資格情報を取得するには、 Marketoソースコネクタの認証 ](./marketo-auth.md) ドキュメントで説明されている手順を参照してください。[

## エクスペリエンスデータモデル（XDM）

XDM は公に文書化された仕様で、サードパーティのソースからデータを取り込んでダウンストリームの Platform サービスで使用するための共通の構造と定義を提供します。

XDM 標準に準拠することで、データを Platform エコシステムに均等に組み込むことができ、データの配信と情報の収集が容易になります。

XDM と Platform での役割について詳しくは、「[XDM システムの概要 ](../../../../xdm/home.md)」を参照してください。

## [!DNL Marketo] から XDM へのフィールドマッピング

[!DNL Marketo] と Platform の間のソース接続を確立するには、Platform に取り込む前に、Marketoのソースデータフィールドを適切なターゲット XDM フィールドにマッピングする必要があります。

[!DNL Marketo] データセットと Platform の間のフィールドマッピングルールについて詳しくは、次を参照してください。

* [アクティビティ](../mapping/marketo.md#activities)
* [プログラム](../mapping/marketo.md#programs)
* [プログラムのメンバーシップ](../mapping/marketo.md#program-memberships)
* [会社](../mapping/marketo.md#companies)
* [静的リスト](../mapping/marketo.md#static-lists)
* [静的リストのメンバーシップ](../mapping/marketo.md#static-list-memberships)
* [名前付きアカウント](../mapping/marketo.md#named-accounts)
* [機会](../mapping/marketo.md#opportunities)
* [商談の連絡先の役割](../mapping/marketo.md#opportunity-contact-roles)
* [人](../mapping/marketo.md#persons)

## Platform での [!DNL Marketo] データの予想される遅延

次の表に、取り込みの性質と目的の宛先に基づいて、[!DNL Marketo] データを Platform に取り込む際に予想される遅延を示します。

| 宛先 | 予想される遅延 |
| ----------- | ---------------- |
| [!DNL Real-time Customer Profile] | &lt; 1 分 |
| データレイク | &lt; 60 分 |

## 次の手順とその他のリソース

次のドキュメントでは、[!DNL Marketo] ソース接続の作成に関する詳細情報を提供します。

* [!DNL Marketo] データを Platform に接続する方法について詳しくは、[UI でのMarketoソースコネクタの作成 ](../../../tutorials/ui/create/adobe-applications/marketo.md) に関するチュートリアルを参照してください。
* [!DNL Marketo] で使用される B2B 名前空間とスキーマの基本的な設定については、[B2B 名前空間とスキーマ ](./marketo-namespaces.md) のドキュメントを参照してください。
* [!DNL Marketo] マンチキン ID の検索と資格情報の生成について詳しくは、[[!DNL Marketo]  認証ガイド ](./marketo-auth.md) を参照してください。
* [!DNL Marketo] データセットに適用される特定のマッピングルールについて詳しくは、[[!DNL Marketo]  フィールドマッピング ](../mapping/marketo.md) に関するドキュメントを参照してください。
