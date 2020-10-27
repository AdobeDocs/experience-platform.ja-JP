---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート（2019 年 12 月 12 日）
doc-type: release notes
last-update: December 12, 2019
author: ens71067
translation-type: tm+mt
source-git-commit: 801da8a705360688f230eae5772a8bed9a1e856e
workflow-type: tm+mt
source-wordcount: '656'
ht-degree: 65%

---


# Adobe Experience Platform リリースノート

**リリース日：2019 年 12 月 12 日**

Adobe Experience Platform の既存の機能のアップデート：

* [[!DNL Segmentation Service]](#segmentation)
* [[!DNL Decisioning Service]](#decisioning)
* [[!DNL Sources]](#sources)
* [[!DNL Experience Data Model (XDM) System]](#xdm)

## [!DNL Segmentation Service] {#segmentation}

Adobe Experience Platform Segmentation Service provides a user interface and RESTful API that allows you to build segments and generate audiences from your [!DNL Real-time Customer Profile] data. These segments are centrally configured and maintained on [!DNL Platform], making them readily accessible by any Adobe application.

[!DNL Segmentation Service] 顧客ベース内のマーケティング可能な人々のグループを区別する基準を説明することで、特定のプロファイルのサブセットを定義します。 セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
|--- | ---|
| 結合されたオーディエンスタブ( [!DNL Segment Builder] | The [!UICONTROL Segments] and [!UICONTROL Audiences] tabs in the [!DNL Segment Builder] have been combined into a single [!UICONTROL Audiences] tab. このタブでは、既存のオーディエンスを参照して検索し、ルールビルダーキャンバスにドラッグ&amp;ドロップして、新しいセグメント定義を作成できます。オーディエンスを参照すると、ルールとしてのオーディエンスのメンバーシップ、または参照先のメンバーシップを定義したルールロジックの完全なオーディエンスのルールロジックのセットのいずれかを新しいセグメント定義に追加できます。 |
| 結合ポリシーセレクターの新しい場所 | The location of the merge policy selector in the [!DNL Segment Builder] has been changed. セグメント定義の結合ポリシーを選択するには、「**[!UICONTROL フィールド]**」タブの歯車アイコンをクリックし、**[!UICONTROL 結合ポリシー]**&#x200B;ドロップダウンメニューを使用して、使用する結合ポリシーを選択します。 |

**既知の問題**

* なし

詳しくは、「[セグメント化サービスの概要](../../segmentation/home.md)を参照してください」。

## [!DNL Decisioning Service] {#decisioning}

Adobe Experience Platform [!DNL Decisioning Service] provides the ability to programmatically and intelligently select the &quot;Next Best Experience&quot; from a set of available options for a given individual, deliver them to any channel or application, and perform reporting and analysis.

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| ランキング関数 | プロファイルのオファーの順序は、すべてのオファーの固定セットではなく、ランキング関数を通じて導き出されるようになりました。 |

**既知の問題**

* なし。

## [!DNL Sources] {#sources}

Adobe Experience Platform can ingest data from external sources while allowing you to structure, label, and enhance that data using [!DNL Platform] services. アドビソリューション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、ストレージシステムと CRM サービスに対する認証、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ---------- | ------------ |
| ストリーミング接続 | Streaming ingestion enables you to send data from client- and server-side devices to [!DNL Experience Platform] in real-time. リリースには、新しいストリーミング接続のユーザーインターフェイスが含まれています。 |
| コネクタのサポート [!DNL Google Cloud Store] | Support for collecting data from [!DNL Google Cloud Store]. |

**既知の問題**

* なし。

ソースについて詳しくは、「[ソースの概要](../../sources/home.md)」を参照してください。

## [!DNL Experience Data Model] (XDM)システム {#xdm}

Standardization and interoperability are key concepts behind [!DNL Experience Platform]. [!DNL Experience Data Model] (XDM)は、Adobeに基づいて、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

XDM はパブリックに文書化された仕様であり、デジタルエクスペリエンスのパワーを向上させるために設計されています。Adobe Experience Platform 上のサービスと通信するすべてのアプリケーションに共通の構造と定義を提供します。XDM 標準規格に準拠することで、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
|--- | ---|
| スキーマ検証の改善 | 参照が期待どおりに追加のフィールドに解決されることを確認するための新しいチェックを追加しました。オブジェクトが完全に定義されていることを確認するため、オブジェクトの配列として定義されたフィールドに対するチェックを追加しました。問題の特定と修正に役立つエラーメッセージを改善しました。 |

**バグの修正**

* アクセス制御とサンドボックスに関連するメンテナンスと改善。
* Support for `eTag` for the `/descriptors` endpoint in the [!DNL Schema Registry] API.

**既知の問題**

* なし

To learn more about working with XDM using the [!DNL Schema Registry] API and [!DNL Schema Editor] user interface, please read the [XDM System documentation](../../xdm/home.md).