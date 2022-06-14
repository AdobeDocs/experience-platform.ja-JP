---
keywords: Experience Platform；ホーム；人気の高いトピック；Marketo Engage;marketo engage;marketo
solution: Experience Platform
title: Marketo Engage コネクタ
topic-legacy: overview
description: このドキュメントでは、Marketo Engage、マッピング、データ遅延に関する情報など、認証ソースコネクタの概要を説明します。
exl-id: 063ec5d9-d643-4141-bf6d-878273f22b33
source-git-commit: 1ecdd5f058a5996b4a3d12ba62c5f352633cd75a
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 14%

---

# [!DNL Marketo Engage] コネクタ

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[[!DNL Marketo Engage]](https://www.marketo.com/software/) （以下「」という。）[!DNL Marketo]&quot;) は、複雑な購入ジャーニーの各ステージを通じてエンゲージメントをおこなうことで顧客体験を変えようとしているリード管理や B2B マーケター向けの完全なソリューションです。

を使用 [!DNL Marketo] ソースコネクタを使用すると、B2B データを [!DNL Marketo] Platform に接続されたアプリケーションを使用して、このデータを Platform に保存し、最新の状態に保ちます。

このドキュメントでは、 [!DNL Marketo] ソースコネクタ（コネクタの認証方法、マッピング方法に関する情報を含む） [!DNL Marketo] Experience Data Model(XDM) に対するフィールドと、コネクタのデータ待ち時間。

## の認証 [!DNL Marketo] コネクタ

接続するには [!DNL Marketo] を Platform に対して設定する場合、まず `munchkinId`, `clientId`、および `clientSecret`.

詳しくは、 [Marketoソースコネクタの認証](./marketo-auth.md) ドキュメントを使用して認証情報を取得します。

## 組織マッピングAdobeの設定

のマッピングセットを確立する前に [!DNL Marketo]を設定する場合は、まず組織マッピングをAdobeする必要があります。 これを完了する方法について詳しくは、 [のAdobe組織マッピングの設定 [!DNL Marketo]](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/miscellaneous/set-up-adobe-organization-mapping.html).

## B2B 名前空間とスキーマ自動生成ユーティリティの設定

次に、B2B 名前空間とスキーマ自動生成ユーティリティを使用して、Platform 開発者コンソールとPostman環境を設定します。 これにより、B2B 名前空間とスキーマを自動入力できます。 詳しい手順については、 [B2B 名前空間とスキーマ自動生成ユーティリティの設定](./marketo-namespaces.md)

## エクスペリエンスデータモデル（XDM）

XDM は公に文書化された仕様で、サードパーティのソースからデータを取り込んでダウンストリームの Platform サービスで使用するための共通の構造と定義を提供します。

XDM 標準に準拠することで、データを Platform エコシステムに均等に組み込むことができ、データの配信と情報の収集が容易になります。

XDM と Platform での役割について詳しくは、 [XDM システムの概要](../../../../xdm/home.md).

## 次のフィールドマッピング [!DNL Marketo] XDM に

間にソース接続を確立するには [!DNL Marketo] と Platform で、Platform に取り込む前に、Marketoのソースデータフィールドを適切なターゲット XDM フィールドにマッピングする必要があります。

次の間のフィールドマッピングルールの詳細については、 [!DNL Marketo] データセットとプラットフォーム：

* [アクティビティ](../mapping/marketo.md#activities)
* [プログラム](../mapping/marketo.md#programs)
* [プログラムのメンバーシップ](../mapping/marketo.md#program-memberships)
* [会社](../mapping/marketo.md#companies)
* [静的リスト](../mapping/marketo.md#static-lists)
* [静的リストのメンバーシップ](../mapping/marketo.md#static-list-memberships)
* [指定顧客](../mapping/marketo.md#named-accounts)
* [商談](../mapping/marketo.md#opportunities)
* [商談連絡先の役割](../mapping/marketo.md#opportunity-contact-roles)
* [人物](../mapping/marketo.md#persons)

## の予想遅延 [!DNL Marketo] Platform のデータ

次の表に、 [!DNL Marketo] 取り込みの特性と目的の宛先に基づいて、Platform にデータを取り込みます。

| 宛先 | 予想される遅延 |
| ----------- | ---------------- |
| [!DNL Real-time Customer Profile] | &lt; 1 分 |
| データレイク | &lt; 60 分 |

## 次の手順と追加のリソース

次のドキュメントでは、 [!DNL Marketo] ソース接続：

* 接続方法に関する情報 [!DNL Marketo] データを Platform に送信する場合は、 [UI でのMarketoソースコネクタの作成](../../../tutorials/ui/create/adobe-applications/marketo.md).
* B2B 名前空間と、で使用されるスキーマの基になる設定について説明します。 [!DNL Marketo]詳しくは、 [B2B 名前空間とスキーマ](./marketo-namespaces.md).
* の検索に関する情報 [!DNL Marketo] munchkin ID と資格情報の生成については、 [[!DNL Marketo] 認証ガイド](./marketo-auth.md).
* に適用される特定のマッピングルールの詳細 [!DNL Marketo] データセットについては、 [[!DNL Marketo] フィールドマッピング](../mapping/marketo.md).
* に関する一般情報 [!DNL Real-time Customer Data Platform B2B Edition] およびその機能については、 [[!DNL Real-time Customer Data Platform B2B Edition]](../../../../rtcdp/b2b-overview.md).
