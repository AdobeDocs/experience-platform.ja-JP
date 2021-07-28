---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2021年7月29日）
doc-type: release notes
last-update: July 28, 2021
author: ens60013
exl-id: 8f2c9bf8-1487-46e4-993b-bd9b63774cab
source-git-commit: ab868a813815e10b520cda2a0abe76e3acdd2ac6
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 53%

---


# Adobe Experience Platform リリースノート

**リリース日：2021 年 7 月 28 日**

Adobe Experience Platform の既存の機能のアップデート：

- [Data Science Workspace](#dsw)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [ソース](#sources)

## Data Science Workspace {#dsw}

Data Science Workspace は、機械学習と人工知能を使用して、データからインサイトを作成します。Adobe Experience Platform に統合された Data Science Workspace は、アドビソリューションでコンテンツやデータアセットを使用して予測をおこなうことを支援します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| ライブラリとOSのアップデート | Data Science Workspaceは、機能と操作性を向上させるために、重要なライブラリとOSの更新をおこないました。 これには、JupyterLab 1.2.20、Python 3.7、Pandas 1.2.4、CUDA 11およびCUDN 8をサポートするTensorflow 2.4.1などが含まれます。 JupyterLab内で使用可能なライブラリを表示する方法については、JupyterLabノートブックの概要ドキュメントの[サポートされるライブラリ](../../data-science-workspace/jupyterlab/overview.md#supported-libraries)の節を参照してください。 |

Data Science Workspace の一般的な情報については、[Data Science Workspace の概要](../../data-science-workspace/home.md)を参照してください。

## 宛先 {#destinations}

宛先は、Adobe Experience Platformからのデータのシームレスなアクティブ化を可能にする、宛先プラットフォームとの事前定義済みの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| [増分ファイルエクスポートの高速化](../../destinations/ui/activate-destinations.md#export-incremental-files) | 3、6、8、12時間ごとに、ファイルベースの宛先の増分ファイル書き出しをスケジュールできるようになりました。 既に保存されているセグメントのファイルエクスポートスケジュールの変更は、現在サポートされていません。 別のスケジュールでセグメントを再書き出しするには、新しい宛先インスタンスを作成する必要があります。 これは、今後のリリースで対処される予定の制限です。 |
| [重複排除キーのサポート](../../destinations/ui/activate-destinations.md#deduplication-keys) | 重複排除キーを選択することで、エクスポートファイル内の同じプロファイルの複数のレコードを排除します。 重複排除キーとして、1つの名前空間または最大2つのXDMスキーマ属性を選択できます。 |

## エクスペリエンスデータモデル（XDM） {#xdm}

エクスペリエンスデータモデル（XDM）は、デジタルエクスペリエンスを向上するよう設計されたオープンソースの仕様です。スキーマの形式のデータに対して共通の構造と定義を提供し、任意のアプリケーションがPlatformサービスと通信できるようにします。

**新機能**

| 機能 | 説明 |
| --- | --- |
| 通信業界のフィルター | UIでスキーマにフィールドグループを追加する際に、通信業界でフィルタリングできるようになりました。 通信の使用例に推奨されるデータモデルについては、[通信業界のエンティティ関係図(ERD)](../../xdm/schema/industries/telecom.md)を参照してください。 |

PlatformのXDMに関する一般的な情報については、「[XDMシステムの概要](../../xdm/home.md)」を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

| 機能 | 説明 |
| ------- | ----------- |
| ベータ版ソースの一般公開（GA） | 以下のソースがベータ版から一般公開（GA）に昇格しました。 <ul><li>[[!DNL Amazon Redshift]](../../sources/connectors/databases/redshift.md)</li><li>[[!DNL Azure Table Storage]](../../sources/connectors/databases/ats.md)</li><li>[[!DNL PayPal]](../../sources/connectors/payments/paypal.md)</li></ul> |
| [!DNL Salesforce Marketing Cloud] （ベータ版） | [!DNL Flow Service] API または UI を使用して、[!DNL Salesforce Marketing Cloud] を Experience Platform に接続できるようになりました。詳しくは、[[!DNL Salesforce Marketing Cloud] コネクタの概要](../../sources/connectors/marketing-automation/salesforce-marketing-cloud.md)を参照してください。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
