---
keywords: Experience Platform；ホーム；人気のトピック；Marketo Engage;marketo engage;marketo
solution: Experience Platform
title: Marketo Engage コネクタ
description: このドキュメントでは、認証、マッピング、データ待ち時間に関する情報など、Marketo Engage ソースコネクタの概要を説明します。
exl-id: 063ec5d9-d643-4141-bf6d-878273f22b33
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '742'
ht-degree: 7%

---

# [!DNL Marketo Engage] コネクタ

>[!IMPORTANT]
>
>Amazon Web Services（AWS）でAdobe Experience Platformを実行するときに、[!DNL Marketo Engage] ソースを使用できるようになりました。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../../landing/multi-cloud.md) を参照してください。

Adobe Experience Platformを使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[[!DNL Marketo Engage]](https://www.marketo.com/software/) は、複雑な購入ジャーニーのすべてのステージにわたるエンゲージメントを通じてカスタマーエクスペリエンスを変革しようとしている経営陣や B2B マーケター向けの完全なソリューションです。

[!DNL Marketo Engage] ソースコネクタを使用すると、[!DNL Marketo Engage] からExperience Platformに B2B データを取り込み、Experience Platformに接続したアプリケーションを使用してこのデータを最新の状態に保つことができます。

>[!IMPORTANT]
>
>[ リアルタイム顧客プロファイル ](../../../../rtcdp/b2b-overview.md) を使用したセグメント化にすべてのMarketo データセットを使用するには、[Adobe Real-Time Customer Data Platform B2B edition](../../../../profile/home.md) へのアクセス権が必要です。 Real-Time CDP B2B editionがない場合でも、Marketo ソースを使用して、人物およびアクティビティデータセットからリアルタイム顧客プロファイルにデータを取り込み、セグメント化できます。

このドキュメントでは、コネクタの認証方法、[!DNL Marketo Engage] フィールドをエクスペリエンスデータモデル（XDM）にマッピングする方法、コネクタのデータ待ち時間など、[!DNL Marketo Engage] ソースコネクタの概要を説明します。

## Adobe組織マッピングの設定

[!DNL Marketo Engage] のマッピングセットを設定する前に、まずAdobe組織マッピングを設定する必要があります。 この完了方法の手順について詳しくは、[Adobe組織マッピングの設定  [!DNL Marketo Engage]](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/miscellaneous/set-up-adobe-organization-mapping.html?lang=ja) を参照してください。

## [!DNL Marketo Engage] コネクタの認証

[!DNL Marketo Engage] をExperience Platformに接続するには、まず `munchkinId`、`clientId`、`clientSecret` の値を取得する必要があります。

資格情報を取得するには、[Marketo ソースコネクタの認証 ](./marketo-auth.md) ドキュメントで説明されている手順を参照してください。

## B2B 名前空間とスキーマ自動生成ユーティリティの設定

次に、B2B 名前空間とスキーマ自動生成ユーティリティを使用して、Experience Platform開発者コンソールとPostman環境を設定します。 これにより、B2B 名前空間とスキーマに自動入力できます。 手順について詳しくは、[B2B 名前空間とスキーマ自動生成ユーティリティの設定 ](./marketo-namespaces.md) に関するガイドを参照してください。

## エクスペリエンスデータモデル（XDM）

XDM は公開ドキュメント化された仕様で、ダウンストリーム Experience Platform サービスで使用するためにサードパーティソースからデータを取り込むことができる共通の構造と定義を提供します。

XDM 標準規格に準拠することで、データをExperience Platform エコシステムに均一に組み込むことができ、データの配信と情報収集が容易になります。

XDM とExperience Platformでの役割について詳しくは、[XDM システムの概要 ](../../../../xdm/home.md) を参照してください。

## [!DNL Marketo Engage] から XDM へのフィールドマッピング

[!DNL Marketo Engage] とExperience Platformの間にソース接続を確立するには、Marketo ソースデータフィールドをExperience Platformに取り込む前に、適切なターゲット XDM フィールドにマッピングする必要があります。

データセットとExperience Platform間のフィールドマッピングルールについて詳 [!DNL Marketo Engage] くは、次を参照してください。

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

## Experience Platformでの [!DNL Marketo Engage] データの予想される待ち時間

次の表に、取り込みの特性と目的の宛先に基づいて、Experience Platformに [!DNL Marketo Engage] データを取り込むために必要な待ち時間の概要を示します。

| 宛先 | 予想される遅延 |
| ----------- | ---------------- |
| [!DNL Real-Time Customer Profile] | &lt; 10 分 |
| データレイク | &lt; 60 分 |

>[!NOTE]
>
>上記の待ち時間の数値は、95% の信頼性レベルでの期待を表しています。 実際の待ち時間は状況によって異なり、まれに 50% 長くなる場合があります。

## 次の手順とその他のリソース

次のドキュメントでは、[!DNL Marketo Engage] ソース接続の作成に関する詳細情報を提供します。

* [!DNL Marketo Engage] データをExperience Platformに接続する方法について詳しくは、[UI でのソース接続の作成 ](../../../tutorials/ui/create/adobe-applications/marketo.md) に関するチュー  [!DNL Marketo Engage]  リアルを参照してください。
   * スキーマの設定とカスタムアクティビティデータの取り込み方法について詳しくは、[ カスタムアクティビティデータのソース接続とデータフローの作成 ](../../../tutorials/ui/create/adobe-applications/marketo-custom-activities.md) に関するチュ  [!DNL Marketo Engage]  トリアルを参照してください
   * [!DNL Person] データセットから [!DNL Activity] データセットに ECID マッピングを移行する方法について詳しくは、[ECID マッピング移行ガイド ](./migration.md) を参照してください。
* [!DNL Marketo Engage] で使用される B2B 名前空間とスキーマの基盤となる設定について詳しくは、[B2B 名前空間とスキーマ ](./marketo-namespaces.md) のドキュメントを参照してください。
* [!DNL Marketo Engage] munchkin ID の検索と資格情報の生成について詳しくは、[[!DNL Marketo Engage]  認証ガイド ](./marketo-auth.md) を参照してください。
* [!DNL Marketo Engage] のデータセットに適用される特定のマッピングルールについて詳しくは、[[!DNL Marketo Engage]  フィールドマッピング ](../mapping/marketo.md) に関するドキュメントを参照してください。
* [!DNL Real-Time Customer Data Platform B2B Edition] とその機能に関する一般的な情報については、[[!DNL Real-Time Customer Data Platform B2B Edition]](../../../../rtcdp/b2b-overview.md) のドキュメントを参照してください。
