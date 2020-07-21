---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2019年12月12日）
doc-type: release notes
last-update: December 12, 2019
author: ens71067
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '668'
ht-degree: 5%

---


# Adobe Experience Platform リリースノート

**リリース日： 2019年12月12日**

Adobe Experience Platform内の既存の機能の更新：

* [!DNL Segmentation Service](#segmentation)
* [!DNL Decisioning Service](#decisioning)
* [!DNL Sources](#sources)
* [!DNL Experience Data Model (XDM) System](#xdm)

## [!DNL Segmentation Service] {#segmentation}

Adobe Experience Platformセグメントサービスは、セグメントを作成して [!DNL Real-time Customer Profile] データからオーディエンスを生成できるユーザーインターフェイスおよびRESTful APIを提供します。 これらのセグメントは一元的に設定および管理され [!DNL Platform]、アドビの任意のアプリケーションから容易にアクセスできます。

[!DNL Segmentation Service] 顧客ベース内のマーケティング可能な人々のグループを区別する基準を説明することで、特定のプロファイルのサブセットを定義します。 セグメントは、記録データ（人口統計情報など）や、ブランドに対する顧客のインタラクションを表す時系列イベントに基づくことができます。

**新機能**

| 機能 | 説明 |
|--- | ---|
| 結合されたオーディエンスタブ( [!DNL Segment Builder] | の「 [!UICONTROL _セグメント&#x200B;_]」タブと「[!UICONTROL _オーディエンス_] 」タブ [!DNL Segment Builder] は、単一の [!UICONTROL _オーディエンス&#x200B;_]タブに結合されました。 このタブでは、既存のオーディエンスを参照および検索できます。既存のセグメントは、ルールビルダーキャンバスにドラッグ&amp;ドロップして、新しいセグメント定義を作成できます。 オーディエンスを参照することで、新しいセグメント定義に次のルールロジックのいずれかを追加できます。 オーディエンスのメンバーシップを規則として指定した場合、参照先のオーディエンスを定義した完全なルールロジックのセットです。 |
| マージポリシーセレクターの新しい場所 | 内のマージポリシーセレクターの場所 [!DNL Segment Builder] が変更されました。 セグメント定義のマージポリシーを選択するには、「 [!UICONTROL _フィールド&#x200B;_]」タブの歯車アイコンをクリックし、「_[!UICONTROL &#x200B;マージポリシー]_ 」ドロップダウンメニューを使用して、使用するマージポリシーを選択します。 |

**既知の問題**

* None

詳しくは、 [Segmentation Serviceの概要を参照してください](../../segmentation/home.md)。

## [!DNL Decisioning Service] {#decisioning}

Adobe Experience Platform [!DNL Decisioning Service] は、特定の個人に対して利用可能な一連のオプションから「次の最高のエクスペリエンス」をプログラム的かつインテリジェントに選択し、チャネルやアプリケーションに配信し、レポートや分析を実行する機能を提供します。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| ランキング関数 | プロファイルのオファーの順序は、すべてのプロファイルにわたる固定のオファーセットではなく、ランキング関数によって導き出されるようになりました。 |

**既知の問題**

* None.

このサービスの詳細については、 [Decisioningサービスの概要](../../decisioning-service/home.md) を参照してください。

## [!DNL Sources] {#sources}

Adobe Experience Platformは、外部ソースからデータを取り込みながら、サー [!DNL Platform] ビスを使用してデータの構造化、ラベル付け、拡張を行うことができます。 アドビのソリューション、クラウドベースのストレージ、サードパーティのソフトウェア、CRMシステムなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] には、様々なデータプロバイダーのソース接続を簡単に設定できる、RESTful APIとインタラクティブUIが用意されています。 これらのソース接続を使用すると、ストレージシステムとCRMサービスに対する認証、取り込みの実行時間の設定、データ取り込みスループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
| ---------- | ------------ |
| ストリーミング接続 | ストリーミング取り込みを使用すると、クライアントおよびサーバ側のデバイスからデータをリアルタイム [!DNL Experience Platform] に送信できます。 リリースには、新しいストリーミング接続ユーザーインターフェイスが含まれています。 |
| コネクタのサポート [!DNL Google Cloud Store] | からのデータ収集のサポート [!DNL Google Cloud Store]。 |

**既知の問題**

* None.

For more information about sources, see the [sources overview](../../sources/home.md).

## [!DNL Experience Data Model] (XDM)システム {#xdm}

標準化と相互運用性は、重要な概念 [!DNL Experience Platform]です。 [!DNL Experience Data Model] (XDM)は、アドビが推進する機能で、カスタマーエクスペリエンスデータを標準化し、カスタマーエクスペリエンス管理のスキーマを定義するための取り組みです。

XDMは、デジタルエクスペリエンスのパワーを向上させるために設計された、公開された仕様です。 Adobe Experience Platform上のサービスと通信するアプリケーションの共通の構造と定義を提供します。 XDM標準を守ることで、すべての顧客体験データを共通の表現に組み込むことができ、より迅速で統合的な方法でインサイトを提供できます。 顧客のアクションから貴重なインサイトを得たり、セグメントを通して顧客オーディエンスを定義したり、顧客属性を使用してパーソナライズを図ることができます。

**新機能**

| 機能 | 説明 |
|--- | ---|
| スキーマ検証の強化 | 新しいチェックを行い、参照が期待どおりに追加のフィールドに解決されることを確認します。 オブジェクトが完全に定義されていることを確認するため、オブジェクトの配列として定義されたフィールドに追加のチェック機能を追加しました。 問題の特定と修正に役立つエラーメッセージを改善。 |

**バグの修正**

* アクセス制御およびサンドボックスに関するメンテナンスと改善。
* APIでの `eTag` エンドポイント `/descriptors`[!DNL Schema Registry] のサポートを追加しました。

**既知の問題**

* None

APIとユーザーインターフェイスを使用したXDMの操作について詳しくは、 [!DNL Schema Registry] XDMシステムのドキュメントを読んでください [!DNL Schema Editor][](../../xdm/home.md)。