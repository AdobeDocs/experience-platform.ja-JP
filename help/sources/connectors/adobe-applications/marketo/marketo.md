---
keywords: Experience Platform；ホーム；人気のトピック；Marketo Engage;marketo engage;marketo
solution: Experience Platform
title: Marketo Engage コネクタ
description: このドキュメントでは、認証、マッピング、データ待ち時間に関する情報を含め、Marketo Engageソースコネクタの概要を説明します。
exl-id: 063ec5d9-d643-4141-bf6d-878273f22b33
source-git-commit: 0c695e11e7d7c14ef7e047cd007668e1099bf127
workflow-type: tm+mt
source-wordcount: '683'
ht-degree: 11%

---

# [!DNL Marketo Engage] コネクタ

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[[!DNL Marketo Engage]](https://www.marketo.com/software/) は、複雑な購入ジャーニーのすべてのステージにわたるエンゲージメントを通じてカスタマーエクスペリエンスを変革しようとしている経営陣や B2B マーケター向けの完全なソリューションです。

（を使用） [!DNL Marketo Engage] ソースコネクタ。から B2B データを取り込むことができます [!DNL Marketo Engage] を Platform に送信し、Platform に接続されたアプリケーションを使用してこのデータを最新の状態に保ちます。

>[!IMPORTANT]
>
>次へのアクセス権が必要です。 [Adobe Real-time Customer Data Platform B2B エディション](../../../../rtcdp/b2b-overview.md) を使用したセグメント化にすべてのMarketo データセットを使用するには [リアルタイム顧客プロファイル](../../../../profile/home.md). Real-Time CDP B2B Edition がない場合でも、Marketo ソースを使用して、人物やアクティビティのデータセットからリアルタイム顧客プロファイルにデータを取り込んでセグメント化できます。

このドキュメントでは、の概要を説明します [!DNL Marketo Engage] ソースコネクタ （コネクタの認証方法とマッピング方法に関する情報を含む） [!DNL Marketo Engage] エクスペリエンスデータモデル（XDM）へのフィールド、およびコネクタのデータ待ち時間。

## Adobe組織マッピングの設定

のマッピングセットを確立する前に [!DNL Marketo Engage]最初にAdobe組織マッピングを設定する必要があります。 この完了方法に関する詳細な手順については、のガイドを参照してください [のAdobe組織マッピングの設定 [!DNL Marketo Engage]](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/miscellaneous/set-up-adobe-organization-mapping.html).

## の認証 [!DNL Marketo Engage] コネクタ

接続するには [!DNL Marketo Engage] platform に移動するには、最初にの値を取得する必要があります `munchkinId`, `clientId`、および `clientSecret`.

で説明されている手順を参照してください。 [Marketo ソースコネクタの認証](./marketo-auth.md) のドキュメントを参照して、資格情報を取得します。

## B2B 名前空間とスキーマ自動生成ユーティリティの設定

次に、B2B 名前空間とスキーマ自動生成ユーティリティを使用して、Platform デベロッパーコンソールとPostman環境を設定します。 これにより、B2B 名前空間とスキーマに自動入力できます。 手順について詳しくは、のガイドを参照してください [b2B 名前空間とスキーマ自動生成ユーティリティの設定](./marketo-namespaces.md)

## エクスペリエンスデータモデル（XDM）

XDM は公開ドキュメント化された仕様で、ダウンストリーム Platform サービスで使用するためにサードパーティソースからデータを取り込むことができる共通の構造および定義を提供します。

XDM 標準規格に準拠することで、データを Platform エコシステムに均一に組み込むことができるため、データの配信と情報の収集が容易になります。

XDM と Platform での役割について詳しくは、を参照してください。 [XDM システムの概要](../../../../xdm/home.md).

## からのフィールドマッピング [!DNL Marketo Engage] から XDM へ

間のソース接続を確立するには [!DNL Marketo Engage] および Platform では、Marketo ソースデータフィールドを Platform に取り込む前に、適切なターゲット XDM フィールドにマッピングする必要があります。

間のフィールドマッピングルールについて詳しくは、次を参照してください [!DNL Marketo Engage] データセットとプラットフォーム：

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

## の予想待ち時間 [!DNL Marketo Engage] platform 上のデータ

次の表に、を取り込むための予想される待ち時間の概要を示します [!DNL Marketo Engage] 取り込みの特性と目的の宛先に基づいて、データを Platform に取り込みます。

| 宛先 | 予想される遅延 |
| ----------- | ---------------- |
| [!DNL Real-Time Customer Profile] | &lt; 10 分 |
| データレイク | &lt; 60 分 |

>[!NOTE]
>
>上記の待ち時間の数値は、95% の信頼性レベルでの期待を表しています。 実際の待ち時間は状況によって異なり、まれに 50% 長くなる場合があります。

## 次の手順とその他のリソース

以下のドキュメントでは、の作成に関する詳細情報を提供します。 [!DNL Marketo Engage] ソース接続：

* を接続する方法について [!DNL Marketo Engage] データを Platform に送信します。に関するチュートリアルを参照してください。 [の作成 [!DNL Marketo Engage] ui のソース接続](../../../tutorials/ui/create/adobe-applications/marketo.md).
   * スキーマの設定方法とカスタムアクティビティデータの取り込み方法について詳しくは、のチュートリアルを参照してください。 [のソース接続とデータフローの作成 [!DNL Marketo Engage] カスタムアクティビティデータ](../../../tutorials/ui/create/adobe-applications/marketo-custom-activities.md)
   * から ECID マッピングを移行する方法について [!DNL Person] データセットをに [!DNL Activity] データセット、を読み取る [ECID マッピング移行ガイド](./migration.md).
* で使用される B2B 名前空間とスキーマの基盤となる設定について詳しくは、 [!DNL Marketo Engage]のドキュメントを参照してください。 [B2B 名前空間とスキーマ](./marketo-namespaces.md).
* の検索について [!DNL Marketo Engage] munchkin ID と認証情報の生成については、を参照してください。 [[!DNL Marketo Engage] 認証ガイド](./marketo-auth.md).
* に適用される特定のマッピングルールについて説明します [!DNL Marketo Engage] データセット、に関するドキュメントを参照してください [[!DNL Marketo Engage] フィールドマッピング](../mapping/marketo.md).
* に関する一般的な情報 [!DNL Real-Time Customer Data Platform B2B Edition] とその機能については、のドキュメントを参照してください。 [[!DNL Real-Time Customer Data Platform B2B Edition]](../../../../rtcdp/b2b-overview.md).
