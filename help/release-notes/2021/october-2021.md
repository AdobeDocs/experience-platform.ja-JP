---
title: Adobe Experience Platform リリースノート 2021年10月
description: Adobe Experience Platform の 2021年10月のリリースノート。
exl-id: 8f8bcb24-6478-4281-9362-9559158384af
source-git-commit: ce967ae176fce81aa26d92b3f0ee8be006808657
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 100%

---

# Adobe Experience Platform リリースノート

**リリース日：2021 年 10 月 27 日**

## Experience Platform に対するアップデート

Experience Platform に対するアップデート。

### ユーザーインターフェイス {#ui}

ユーザーインターフェイスが新しくなり、次の変更が加えられました。

| 機能 | 説明 |
| --- | --- |
| ダークテーマ | ダークテーマスイッチを使用すると、Platform インターフェースのテーマをライト／ダークに切り替えることができます。スイッチは、ユーザープロファイルの、ユーザー名とメールの下にあります。 |
| 左ナビゲーションの切り替え | アプリケーションのヘッダー上部にある改良されたナビゲーショントグルを使用して、Experience Platform の機能を表示するメニューの表示／非表示を切り替えることができます。システムは最後の選択を記憶しており、アクセスできる機能のみを表示します。 |
| アクセス表示 | 左側のナビゲーションバーには、アクセス可能な機能のみが表示されます。 以前のバージョンの Adobe Experience Platform では、使用できない項目が、（アクセスできない場合でも）表示されていました。 |

詳しくは、[Platform UI ガイド](../../landing/ui-guide.md)を参照してください。

## 既存の機能に対するアップデート

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Data Prep]](#data-prep)
- [ソース](#sources)

### [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータをマッピング、変換および検証できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| `contains_key` 関数 | `contains_key` 関数が導入されました。この関数を使用すると、オブジェクトがソース内に存在するかどうかを確認できます。この関数は、現在非推奨となっている `is_set` 関数に代わるものです。 |
| エラーメッセージ | Data Prep API の `/mappingSets/preview` エンドポイントによって返されるエラーメッセージが、実行時に生成されるエラーメッセージと一致するようになりました。 |

このサービスについて詳しくは、[[!DNL Data Prep] 概要](../../data-prep/home.md)を参照してください。

### ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

| 機能 | 説明 |
| --- | --- |
| [!DNL Amazon S3] ソースの機能強化 | これで、`s3SessionToken` パラメーターを使用して、一時的なセキュリティ資格情報を使用して [!DNL Amazon S3] アカウントをプラットフォームに接続できます。このトークンを使用すると、信頼できない環境のユーザーに [!DNL Amazon S3] リソースへの短期間の一時的なアクセスを提供できます。詳しくは、[[!DNL Amazon S3] ドキュメント](../../sources/connectors/cloud-storage/s3.md#prerequisites)を参照してください。 |
| [!DNL Generic REST API]（ベータ版） | [[!DNL Flow Service] API](../../sources/tutorials/api/create/protocols/generic-rest.md) を使用して [!DNL Generic REST API] ソース接続を作成し、汎用の REST アプリケーションから Platform にデータを取り込むことができるようになりました。詳しくは、[[!DNL Generic REST API] 概要](../../sources/connectors/protocols/generic-rest.md)を参照してください。 |
| [!DNL Zoho CRM]（ベータ版） | [[!DNL Flow Service] API](../../sources/tutorials/api/create/crm/zoho.md) または[ユーザーインターフェース](../../sources/tutorials/ui/create/crm/zoho.md)を使用して [!DNL Zoho CRM] ソース接続を作成し、[!DNL Zoho CRM] アカウントから Platform にデータを取り込むことができるようになりました。詳しくは、[[!DNL Zoho CRM]  の概要](../../sources/connectors/crm/zoho.md)を参照してください。 |

ソースについて詳しくは、 [ソースの概要](../../sources/home.md) を参照してください。
