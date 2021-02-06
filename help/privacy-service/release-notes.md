---
keywords: Experience Platform；ホーム；人気の高いトピック
solution: Experience Platform
title: Privacy Serviceリリースノート
topic: release notes
description: Adobe Experience Platform Privacy Serviceの最新のリリースノートです。
translation-type: tm+mt
source-git-commit: f2238d35f3e2a279fbe8ef8b581282102039e932
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 60%

---


# [!DNL Privacy Service] リリースノート

このドキュメントには、Adobe Experience Platform[!DNL Privacy Service]の新機能、および拡張機能と重要なバグ修正に関する情報が含まれています。

>[!NOTE]
>
>他の[!DNL Experience Platform]サービスに関する最新のリリースノートは、[こちら](../release-notes/latest/latest.md)を参照してください。

## 2020 年 9 月 9 日

### 新機能

| 機能 | 説明 |
| --- | --- |
| LGPDのサポート（ブラジル） | ブラジルの[!DNL Lei Geral de Proteção de Dados](LGPD)規制の下で、プライバシージョブを作成できるようになりました。 これらのジョブは、規則コード`lgpd_bra`の下で追跡されます。 |

## 2020年4月8日

### 新機能

| 機能 | 説明 |
| --- | --- |
| PDPA のサポート | [!DNL Privacy] タイのPersonal Data Protection Act(PDPA)に基づいて、リクエストを作成し、追跡できるようになりました。API でプライバシーリクエストを実行する場合、`regulation` 配列は「pdpa_tha」の値を受け取ります。 |
| UI の名前空間タイプ | [!DNL Privacy Service] UIのリクエストビルダーで、異なる名前空間タイプを指定できるようになりました。 詳しくは、[ユーザガイド](ui/user-guide.md)を参照してください。 |
| 古いエンドポイントの廃止 | 古い API エンドポイント（`data/privacy/gdpr`）は非推奨となりました。 |

## 2020年1月14日

### 新機能

| 機能 | 説明 |
| --- | --- |
| [!DNL Privacy Service] 商標変更 | GDPRに加え、他の規制をサポートするようになったため、以前の名称は[!DNL Privacy Service]に変更された。 |
| 新しい API エンドポイント | [!DNL Privacy Service] APIの基本パスが`/data/privacy/gdpr`から`/data/core/privacy/jobs`に更新されました |
| 新しい必須の `regulation` プロパティ | [!DNL Privacy Service] APIで新しいジョブを作成する場合、`regulation`プロパティを要求ペイロードに指定して、ジョブを追跡する規則を示す必要があります。 指定できる値は、`gdpr` および `ccpa` です。詳しくは、『 開発者ガイド』の[プライバシージョブ](api/privacy-jobs.md)に関するドキュメントを参照してください。[!DNL Privacy Service] |
| Adobe Primetime Authentication のサポート | [!DNL Privacy Service] では、製品の値として `primetimeAuthentication` を使用して、Adobe Primetime Authentication からのアクセス / 削除リクエストを受け入れるようになりました。詳しくは、[Primetime 認証のドキュメント](http://tve.helpdocsonline.com/how-to-make-a-privacy-request)を参照してください。 |

### 強化された機能

* [!DNL Privacy Service] UIの強化：
   * GDPR 規制と CCPA 規制下でのジョブトラッキングに関するページが分離されました。
   * GDPR と CCPA 下でのトラッキングデータを切り替えるための「*規制の種類*」ドロップダウンが新しく追加されました。

## 2019年7月25日

### 新機能

| 機能 | 説明 |
| --- | --- |
| リクエスト指標ダッシュボード | [!DNL Privacy Service] UIの新しい指標ダッシュボードは、送信されたGDPR要求、エラー要求、完了したGDPR要求を表示します。 |
| リクエストビルダー | サービス組織の技術ユーザーとそれ以外のユーザーの両方が GDPR リクエストを送信する場合、UI にリクエストを作成する機能が追加されます。JSONファイル送信機能は、引き続き使用したい組織の[!DNL Privacy Service] UIで引き続き利用できます。 |
| GDPR ジョブイベントの通知 | GDPR ジョブのステータスに関するイベント通知は、多くのワークフローにとって重要な要素です。これまで、通知は個々の電子メール通知を使用して提供されていましたが、GDPR イベント通知は Adobe I/O イベントを活用するメッセージです。このメッセージは、設定済みの Webhook に送信されるので、ジョブリクエストの自動化が容易になります。[!DNL Privacy Service] UI ユーザーは、Adobe I/O GDPR イベントを登録することで、製品のジョブまたは GDPR ジョブが完了したときに更新を受け取ることができます。 |

## 2019年4月18日

### 強化された機能

* [!DNL Privacy Service] UIのステータステーブルのデフォルトの範囲が7日間に変更されました。
* 内部例外の処理が改良されました。
* データ変更率が低い一般的な内部呼び出しにキャッシュを導入することで、パフォーマンスが向上しました。

### バグの修正

*  API の `GET /` エンドポイント向けに、フィルターされたクエリの不足ログ情報を追加しました。[!DNL Privacy Service]

## 2019年4月11日

### 強化された機能

* UI が更新され、ベータ版の顧客向けの新機能がサポートされました。
* ベータ版の UI 2.0 機能をサポートする新しい指標 API

## 2019年4月9日

### 強化された機能

* すべての検索（GET）API 呼び出しが更新され、30 日間のルックバック範囲がデフォルトに設定されました。
* API の使用が制限され、ルックバックの最大範囲が 45 日間になりました。

## 2019年2月14日

### 強化された機能

* すべての POST 送信において、`include` フィールドが強制的に使用されます。
* JSON のアップロード時に、`include` フィールドが強制的に使用されます。

### バグの修正

* ユーザーが[!DNL Privacy Service] UIを読み込めない問題を修正しました。