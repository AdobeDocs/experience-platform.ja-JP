---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
source-git-commit: b6f4c79df79ae20b8051b69ef34dd255df193454
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 39%

---

# Adobe Experience Platform リリースノート

**リリース日：2021 年 10 月 27 日（PT）**

## Experience Platformの更新

Experience Platformの更新。

### ユーザーインターフェイス {#ui}

ユーザーインターフェイスが更新され、次の変更が加えられました。

| 機能 | 説明 |
| --- | --- |
| ダークテーマ | 「ダークテーマ」スイッチを使用して、Platform インターフェイスのライトテーマとダークテーマを切り替えます。 スイッチは、ユーザー名と電子メールの下のユーザープロファイルにあります。 |
| 左ナビゲーションを切り替え | アプリケーションヘッダーの上部にある改善されたナビゲーション切り替えを使用して、Experience Platform機能を表示するメニューの表示/非表示を切り替えます。 最後の選択が記憶され、アクセス権のある機能のみが表示されます。 |
| アクセス表示 | 左側のナビゲーションバーには、アクセス可能な機能のみが表示されます。 以前のバージョンのAdobe Experience Platformでは、使用できない項目（アクセスできない場合でも）が表示されていました。 |

詳しくは、 [Platform UI ガイド](../../landing/ui-guide.md) を参照してください。

## 既存の機能の更新

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Prep]](#data-prep)
- [ソース](#sources)

### [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータのマッピング、変換、検証をおこなうことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| `contains_key` 関数 | この `contains_key` 関数が導入されました。この関数を使用すると、オブジェクトがソース内に存在するかどうかを確認できます。 この関数は、 `is_set` 関数内で使用する必要があります。現在は非推奨です。 |
| エラーメッセージ | 次によって返されたエラーメッセージ： `/mappingSets/preview` Data Prep API のエンドポイントが、実行時に生成されるエラーメッセージと一致するようになりました。 |

詳しくは、 [[!DNL Data Prep] 概要](../../data-prep/home.md) このサービスの詳細を確認するには、を参照してください。

### ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

| 機能 | 説明 |
| --- | --- |
| [!DNL Amazon S3] ソースの機能強化 | これで、 `s3SessionToken` 接続するためのパラメーター [!DNL Amazon S3] 一時的なセキュリティ資格情報を使用して、Platform にアカウントを設定します。 このトークンを使用すると、 [!DNL Amazon S3] 信頼できない環境のユーザーに対するリソース。 詳しくは、[[!DNL Amazon S3]  のドキュメント](../../sources/connectors/cloud-storage/s3.md#prerequisites)を参照してください。 |
| [!DNL Generic REST API] （ベータ版） | これで、 [!DNL Generic REST API] を使用したソース接続 [[!DNL Flow Service] API](../../sources/tutorials/api/create/protocols/generic-rest.md) を使用して、汎用の REST アプリケーションから Platform にデータを取り込みます。 詳しくは、[[!DNL Generic REST API]  の概要](../../sources/connectors/protocols/generic-rest.md)を参照してください。 |
| [!DNL Zoho CRM] （ベータ版） | これで、 [!DNL Zoho CRM] を使用したソース接続 [[!DNL Flow Service] API](../../sources/tutorials/api/create/crm/zoho.md) または [ユーザーインターフェイス](../../sources/tutorials/ui/create/crm/zoho.md) データを取り込む [!DNL Zoho CRM] アカウントを Platform に送信します。 詳しくは、[[!DNL Zoho CRM]  の概要](../../sources/connectors/crm/zoho.md)を参照してください。 |

ソースについて詳しくは、 [ソースの概要](../../sources/home.md) を参照してください。
