---
title: Adobe Experience Platform リリースノート 2019 年 12 月
description: Adobe Experience Platformの 2019 年 12 月のリリースノート。
doc-type: release notes
last-update: December 12, 2019
author: ens71067
exl-id: 98d50b90-38ed-4cc2-ad48-78b712b453f7
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '663'
ht-degree: 67%

---

# Adobe Experience Platform リリースノート

**リリース日：2019 年 12 月 11 日**

Adobe Experience Platform の既存の機能に対するアップデート：

* [[!DNL Segmentation Service]](#segmentation)
* [[!DNL Decisioning Service]](#decisioning)
* [[!DNL Sources]](#sources)
* [[!DNL Experience Data Model (XDM) System]](#xdm)

## [!DNL Segmentation Service] {#segmentation}

Adobe Experience Platform セグメント化サービスは、セグメントを作成し、[!DNL Real-Time Customer Profile] データからオーディエンスを生成できるユーザーインターフェイスおよび RESTful API を提供します。これらのセグメントは、[!DNL Experience Platform] 上で一元的に設定および管理され、アドビのアプリケーションから簡単にアクセスできます。

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
|--- | ---|
| [!DNL Segment Builder] の「結合オーディエンス」タブ | [!DNL Segment Builder] ージの [!UICONTROL  セグメント ] タブと [!UICONTROL  オーディエンス ] タブは、1 つの [!UICONTROL  オーディエンス ] タブに結合されました。 このタブでは、既存のオーディエンスを参照して検索し、ルールビルダーキャンバスにドラッグ&amp;ドロップして、新しいセグメント定義を作成できます。オーディエンスを参照すると、ルールとしてのオーディエンスのメンバーシップ、または参照先のメンバーシップを定義したルールロジックの完全なオーディエンスのルールロジックのセットのいずれかを新しいセグメント定義に追加できます。 |
| 結合ポリシーセレクターの新しい場所 | リク [!DNL Segment Builder] スト内の結合ポリシーセレクターの場所が変更されました。 セグメント定義の結合ポリシーを選択するには、「**[!UICONTROL フィールド]**」タブの歯車アイコンを選択し、**[!UICONTROL 結合ポリシー]** ドロップダウンメニューを使用して、使用する結合ポリシーを選択します。 |

**既知の問題**

* なし

詳しくは、「[セグメント化サービスの概要](../../segmentation/home.md)を参照してください」。

## [!DNL Decisioning Service] {#decisioning}

Adobe Experience Platform [!DNL Decisioning Service] は、特定の個人に使用可能な一連のオプションから「次善のエクスペリエンス」をプログラムでインテリジェントに選択し、任意のチャネルやアプリケーションに配信して、レポートと分析を実行する機能を提供します。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| ランキング関数 | プロファイルのオファーの順序は、すべてのオファーの固定セットではなく、ランキング関数を通じて導き出されるようになりました。 |

**既知の問題**

* なし。

## [!DNL Sources] {#sources}

Adobe Experience Platformでは、外部ソースからデータを取り込むときに、[!DNL Experience Platform] サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。 アドビソリューション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を提供します。 これらのソース接続を使用すると、ストレージシステムと CRM サービスに対する認証、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ---------- | ------------ |
| ストリーミング接続 | ストリーミング取り込みでは、クライアントサイドおよびサーバーサイドのデバイスから、リアルタイムで [!DNL Experience Platform] にデータを送信できます。 リリースには、新しいストリーミング接続のユーザーインターフェイスが含まれています。 |
| [!DNL Google Cloud Store] のコネクタサポート | [!DNL Google Cloud Store] からのデータ収集のサポート。 |

**既知の問題**

* なし。

ソースについて詳しくは、「[ソースの概要](../../sources/home.md)」を参照してください。

## [!DNL Experience Data Model]（XDM）システム {#xdm}

標準化と相互運用性は、[!DNL Experience Platform] の背後にある重要な概念です。アドビが推進する [!DNL Experience Data Model]（XDM）は、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

XDM はパブリックに文書化された仕様であり、デジタルエクスペリエンスのパワーを向上させるために設計されています。Adobe Experience Platform 上のサービスと通信するすべてのアプリケーションに共通の構造と定義を提供します。XDM 標準規格に準拠することで、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
|--- | ---|
| スキーマ検証の改善 | 参照が期待どおりに追加のフィールドに解決されることを確認するための新しいチェックを追加しました。オブジェクトが完全に定義されていることを確認するため、オブジェクトの配列として定義されたフィールドに対するチェックを追加しました。問題の特定と修正に役立つエラーメッセージを改善しました。 |

**バグの修正**

* アクセス制御とサンドボックスに関連するメンテナンスと改善。
* [!DNL Schema Registry] API での `/descriptors` エンドポイントの `eTag` のサポート。

**既知の問題**

* なし

[!DNL Schema Registry] API とユーザーインターフェイスを使用した XDM の操作について詳しくは、[XDM システム [!DNL Schema Editor] ドキュメント ](../../xdm/home.md) を参照してください。
