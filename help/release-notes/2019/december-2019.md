---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート（2019 年 12 月 12 日）
doc-type: release notes
last-update: December 12, 2019
author: ens71067
translation-type: tm+mt
source-git-commit: 126b3d1cf6d47da73c6ab045825424cf6f99e5ac
workflow-type: tm+mt
source-wordcount: '657'
ht-degree: 59%

---


# Adobe Experience Platform リリースノート

**リリース日：2019 年 12 月 12 日**

Adobe Experience Platform の既存の機能のアップデート：

* [[!DNL Segmentation Service]](#segmentation)
* [[!DNL Decisioning Service]](#decisioning)
* [[!DNL Sources]](#sources)
* [[!DNL Experience Data Model (XDM) System]](#xdm)

## [!DNL Segmentation Service] {#segmentation}

Adobe Experience Platformセグメントサービスは、セグメントを作成して[!DNL Real-time Customer Profile]データからオーディエンスを生成するためのユーザーインターフェイスおよびRESTful APIを提供します。 これらのセグメントは[!DNL Platform]上で一元的に構成および管理され、どのAdobeアプリケーションでも容易にアクセスできます。

[!DNL Segmentation Service] 顧客ベース内のマーケティング可能な人々のグループを区別する基準を説明することで、特定のプロファイルのサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
|--- | ---|
| [!DNL Segment Builder]のマージされたオーディエンスタブ | [!DNL Segment Builder]の[!UICONTROL セグメント]と[!UICONTROL オーディエンス]タブは、単一の[!UICONTROL オーディエンス]タブに結合されました。 このタブでは、既存のオーディエンスを参照して検索し、ルールビルダーキャンバスにドラッグ&amp;ドロップして、新しいセグメント定義を作成できます。オーディエンスを参照すると、ルールとしてのオーディエンスのメンバーシップ、または参照先のメンバーシップを定義したルールロジックの完全なオーディエンスのルールロジックのセットのいずれかを新しいセグメント定義に追加できます。 |
| 結合ポリシーセレクターの新しい場所 | [!DNL Segment Builder]内の結合ポリシーセレクターの場所が変更されました。 セグメント定義の結合ポリシーを選択するには、「**[!UICONTROL フィールド]**」タブの歯車アイコンを選択し、**[!UICONTROL 結合ポリシー]**&#x200B;ドロップダウンメニューを使用して、使用する結合ポリシーを選択します。 |

**既知の問題**

* なし

詳しくは、「[セグメント化サービスの概要](../../segmentation/home.md)を参照してください」。

## [!DNL Decisioning Service] {#decisioning}

Adobe Experience Platform[!DNL Decisioning Service]は、プログラム的かつインテリジェントに、特定の個人に利用可能な一連のオプションから「次の最高のエクスペリエンス」を選択し、チャネルやアプリケーションに配信し、レポートや分析を実行する機能を提供します。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| ランキング関数 | プロファイルのオファーの順序は、すべてのオファーの固定セットではなく、ランキング関数を通じて導き出されるようになりました。 |

**既知の問題**

* なし。

## [!DNL Sources] {#sources}

Adobe Experience Platformは外部ソースからデータを取り込みながら、[!DNL Platform]サービスを使ってデータの構造、ラベル付け、拡張を行うことができます。 アドビソリューション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、ストレージシステムと CRM サービスに対する認証、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ---------- | ------------ |
| ストリーミング接続 | ストリーミング取り込みを使用すると、クライアント側およびサーバ側のデバイスから[!DNL Experience Platform]にデータをリアルタイムで送信できます。 リリースには、新しいストリーミング接続のユーザーインターフェイスが含まれています。 |
| [!DNL Google Cloud Store]のコネクタサポート | [!DNL Google Cloud Store]からのデータ収集のサポート。 |

**既知の問題**

* なし。

ソースについて詳しくは、「[ソースの概要](../../sources/home.md)」を参照してください。

## [!DNL Experience Data Model] (XDM)システム  {#xdm}

標準化と相互運用性は、[!DNL Experience Platform]の背後にある重要な概念です。 [!DNL Experience Data Model] (XDM)は、Adobeに基づいて、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

XDM はパブリックに文書化された仕様であり、デジタルエクスペリエンスのパワーを向上させるために設計されています。Adobe Experience Platform 上のサービスと通信するすべてのアプリケーションに共通の構造と定義を提供します。XDM 標準規格に準拠することで、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
|--- | ---|
| スキーマ検証の改善 | 参照が期待どおりに追加のフィールドに解決されることを確認するための新しいチェックを追加しました。オブジェクトが完全に定義されていることを確認するため、オブジェクトの配列として定義されたフィールドに対するチェックを追加しました。問題の特定と修正に役立つエラーメッセージを改善しました。 |

**バグの修正**

* アクセス制御とサンドボックスに関連するメンテナンスと改善。
* [!DNL Schema Registry] APIの`/descriptors`エンドポイントの`eTag`をサポートします。

**既知の問題**

* なし

[!DNL Schema Registry] APIと[!DNL Schema Editor]ユーザーインターフェイスを使用したXDMの使い方の詳細については、[XDMシステムドキュメント](../../xdm/home.md)を読んでください。