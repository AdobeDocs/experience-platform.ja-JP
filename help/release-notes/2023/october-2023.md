---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の 2023年10月のリリースノート。
source-git-commit: d024596c7d85139721ef370f9e081911a217d9ba
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 47%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年10月25日（PT）**

 Experience Platform の既存の機能に対するアップデート：

- [ソース](#sources)

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| データランディングゾーンの認証を更新しました | 資格情報の表示時に、データランディングゾーンの指定された有効期限が表示されるようになりました。 アプリケーションでトークンを引き続き使用するには、有効期限の前にトークンを更新する必要があります。 指定した有効期限より前に手動でトークンを更新しない場合、次回資格情報を取得する際に自動的に更新され、新しいトークンが提供されます。 詳しくは、 [データランディングゾーンの使用](../../sources/tutorials/ui/create/cloud-storage/data-landing-zone.md). |

{style="table-layout:auto"}

ソースの詳細については、 [ソースの概要](../../sources/home.md).
