---
keywords: エクスペリエンス Platform、home、人気のある話題。Marketo Marketo 連携; Marketo
solution: Experience Platform
title: Marketo 提携コネクタ
topic-legacy: overview
description: このドキュメントでは、Marketo 連携ソースコネクタの概要について、その認証、マッピング、データの遅延について説明しています。
exl-id: 063ec5d9-d643-4141-bf6d-878273f22b33
source-git-commit: a36a4775c14e97df51f218cea3a083d29c7b69dc
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 13%

---

# (ベータ版) [!DNL Marketo Engage] コネクタ

>[!IMPORTANT]
>
>[!DNL Marketo Engage]現在のところ、Adobe エクスペリエンスプラットフォームのソースがベータ版になっています。ドキュメントと機能は変更される場合があります。

Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[[!DNL Marketo Engage]](https://www.marketo.com/software/) (hereinafter &quot;&quot; と呼ば [!DNL Marketo] れる) は、複雑な購入 journeys のあらゆる段階で、カスタマーエクスペリエンスを変革させたいと考える、リード管理および B2B マーケティング向けの包括的なソリューションです。

[!DNL Marketo]ソースコネクタを使用すると、B2B データ [!DNL Marketo] をプラットフォームに取り込んで、プラットフォームに接続されたアプリケーションを使用してデータを最新の状態に保つことができます。

この記事では [!DNL Marketo] 、コネクタの認証方法、データモデル (XDM) を利用できるようにフィールドをマップする方法、コネクタのデータ潜在期間について、ソースコネクタの概要を説明して [!DNL Marketo] います。

## コネクタの認証 [!DNL Marketo]

プラットフォームに接続するには、まず、、、 [!DNL Marketo] およびの値を取得する必要があり `munchkinId` `clientId` `clientSecret` ます。

[ ](./marketo-auth.md) 資格情報を取得するには、「Marketo ソースコネクターを認証する」で説明されている手順を参照してください。

## Adobe エクスペリエンスクラウドの共有を設定します。

用マッピングセットを確立する前に [!DNL Marketo] 、まず Adobe エクスペリエンスクラウドの共有を設定する必要があります。 これを行う方法について詳しくは、 [ アドビシステムズ社の共有ユーザー向けクラウドの設定についてのガイドを参照してください  [!DNL Marketo] ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/miscellaneous/set-up-adobe-experience-cloud-audience-sharing.html?lang=en) 。

## エクスペリエンスデータモデル（XDM）

XDM は、サードパーティのソースからデータを取り込み、下流のプラットフォームサービスで使用するための一般的な構造と定義を提供する、公開された仕様仕様です。

XDM 規格に準拠しているので、データを均等にプラットフォームに組み込むことができ、データを配信し、情報を収集することが容易になります。

XDM とその役割について詳しくは、xdm システムの概要を参照してください [ ](../../../../xdm/home.md) 。

## フィールドマッピングの場合 [!DNL Marketo] は XDM になります。

And Platform の間にソース接続を確立するには [!DNL Marketo] 、Marketo source データフィールドを、プラットフォームに ingested する前に、適切なターゲット XDM フィールドにマップする必要があります。

データセットとプラットフォーム間のフィールドマッピングルールについて詳しくは、次の項目を参照してください [!DNL Marketo] 。

* [アクティビティ](../mapping/marketo.md#activities)
* [プログラム](../mapping/marketo.md#programs)
* [プログラムメンバーシップ](../mapping/marketo.md#program-memberships)
* [会社](../mapping/marketo.md#companies)
* [静的なリスト](../mapping/marketo.md#static-lists)
* [静的なリストメンバーシップ](../mapping/marketo.md#static-list-memberships)
* [名前付きアカウント](../mapping/marketo.md#named-accounts)
* [パートナー](../mapping/marketo.md#opportunities)
* [営業案件取引先担当者ロール](../mapping/marketo.md#opportunity-contact-roles)
* [人](../mapping/marketo.md#persons)

## プラットフォーム上のデータの予想レイテンシー [!DNL Marketo]

以下の表に、取り込むデータをプラットフォームに取り込む際の予想されるレーテンシーの概略を [!DNL Marketo] 示します。これは、取り込む対象の種類と目的に応じて異なります。

| 宛先 | 予想される遅延 |
| ----------- | ---------------- |
| [!DNL Real-time Customer Profile] | &lt; 1 分 |
| データレイク | &lt; 60 分 |

## 次の手順とその他のリソース

ソース接続の作成について詳しくは、次のマニュアルを参照して [!DNL Marketo] ください。

* データをプラットフォームに接続する方法につい [!DNL Marketo] ては、Marketo source connector FOR UI の作成に関するチュートリアルを参照してください [ ](../../../tutorials/ui/create/adobe-applications/marketo.md) 。
* によって使用される B2B 名前空間およびスキーマの基になる設定について詳しくは [!DNL Marketo] 、 [ b2b 名前空間とスキーマのマニュアルを参照してください ](./marketo-namespaces.md) 。
* Munchkin ID の検索およびサインイン情報の生成について詳しくは、 [!DNL Marketo] 認証ガイドを参照してください [[!DNL Marketo]  ](./marketo-auth.md) 。
* データセットに適用される具体的なマッピングルールについて詳しくは [!DNL Marketo] 、フィールドマッピングに関するドキュメントを参照してください [[!DNL Marketo]  ](../mapping/marketo.md) 。
* 概要とその機能については [!DNL Real-time Customer Data Platform B2B Edition] 、のマニュアルを参照してください [[!DNL Real-time Customer Data Platform B2B Edition]](../../../../rtcdp/b2b-overview.md) 。
