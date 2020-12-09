---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2020年12月9日）
doc-type: release notes
last-update: December 9, 2020
author: ens60013
translation-type: tm+mt
source-git-commit: 25c162f50f0a66d77eb638dbf87893af3c543ddc
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 46%

---


# Adobe Experience Platform リリースノート

**リリース日：2020年12月9日**

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Sources]](#sources)

## [!DNL Sources] {#sources}

Adobe Experience Platform can ingest data from external sources while allowing you to structure, label, and enhance that data using [!DNL Platform] services. アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| ストリーミングソースのアカウントと接続の詳細の更新 | APIとUIを使用して、既存のストリーミング接続の名前、説明および秘密鍵証明書を更新できるようにな [!DNL Flow Service] りました。 詳しくは、APIを使用した接続の [更新とUIを使用したアカウントの詳細の](../../sources/tutorials/api/update.md) 編集に関するチュートリアルを参照してください [](../../sources/tutorials/ui/monitor.md)。 |
| データフローの削除 | エラーを含む、または不要になったストリーミングデータフローを、 [!DNL Flow Service] APIとUIを使用して削除できるようになりました。 詳細は、APIを使用したデータ・フローの [削除およびUIを使用したデータ・フローの](../../sources/tutorials/api/delete-dataflows.md) 削除に関するチュートリアルを参照してください [](../../sources/tutorials/ui/delete.md)。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。