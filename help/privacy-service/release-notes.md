---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Privacy Service のリリースノート
topic: release notes
translation-type: tm+mt
source-git-commit: 6eee7e903d36ed641c9f8e6120f549c02cb4bce4
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 62%

---


# [!DNL Privacy Service] リリースノート

This document contains information about new features for Adobe Experience Platform [!DNL Privacy Service], as well as enhancements and significant bug fixes.

>[!NOTE]
>
>その他の [!DNL Experience Platform] サービスの最新のリリースノートは、 [こちらを参照してください](../release-notes/latest/latest.md)。

## 2020年9月9日

### 新機能

| 機能 | 説明 |
| --- | --- |
| LGPDのサポート（ブラジル） | ブラジル [!DNL Lei Geral de Proteção de Dados] (LGPD)の規制に基づき、プライバシー・ジョブを作成できるようになった。 これらのジョブは、規制コードの下で追跡され `lgpd_bra`ます。 |

## 2020年4月8日

### 新機能

| 機能 | 説明 |
| --- | --- |
| PDPA のサポート | [!DNL Privacy] タイのPersonal Data Protection Act(PDPA)に基づいて、リクエストを作成し、追跡できるようになりました。 API でプライバシーリクエストを実行する場合、`regulation` 配列は「pdpa_tha」の値を受け取ります。 |
| UI の名前空間タイプ | You can now specify different namespace types in the Request Builder in the [!DNL Privacy Service] UI. 詳しくは、[ユーザガイド](ui/user-guide.md)を参照してください。 |
| 古いエンドポイントの廃止 | 古い API エンドポイント（`data/privacy/gdpr`）は非推奨となりました。 |

## 2020年1月14日

### 新機能

| 機能 | 説明 |
| --- | --- |
| [!DNL Privacy Service] 商標変更 | The formerly named &quot;GDPR Service&quot; has been rebranded to [!DNL Privacy Service] as the service has grown to support other regulations in addition to GDPR. |
| 新しい API エンドポイント | Base path for the [!DNL Privacy Service] API has been updated from `/data/privacy/gdpr` to `/data/core/privacy/jobs` |
| 新しい必須の `regulation` プロパティ | When creating new jobs in the [!DNL Privacy Service] API, a `regulation` property must be supplied in the request payload to indicate which regulation to track the job under. 指定できる値は、`gdpr` および `ccpa` です。詳しくは、『 開発者ガイド』の[プライバシージョブ](api/privacy-jobs.md)に関するドキュメントを参照してください。[!DNL Privacy Service] |
| Adobe Primetime Authentication のサポート | [!DNL Privacy Service] では、製品の値として `primetimeAuthentication` を使用して、Adobe Primetime Authentication からのアクセス / 削除リクエストを受け入れるようになりました。詳しくは、[Primetime 認証のドキュメント](http://tve.helpdocsonline.com/how-to-make-a-privacy-request)を参照してください。 |

### 強化された機能

* [!DNL Privacy Service] UIの強化：
   * GDPR 規制と CCPA 規制下でのジョブトラッキングに関するページが分離されました。
   * GDPR と CCPA 下でのトラッキングデータを切り替えるための「_規制の種類_」ドロップダウンが新しく追加されました。

## 2019年7月25日

### 新機能

| 機能 | 説明 |
| --- | --- |
| リクエスト指標ダッシュボード | The new metrics dashboard in the [!DNL Privacy Service] UI provides visibility into submitted, errored, and completed GDPR requests. |
| リクエストビルダー | サービス組織の技術ユーザーとそれ以外のユーザーの両方が GDPR リクエストを送信する場合、UI にリクエストを作成する機能が追加されます。The JSON file submission capability is still available in the [!DNL Privacy Service] UI for those organizations who prefer to continue using it. |
| GDPR ジョブイベントの通知 | GDPR ジョブのステータスに関するイベント通知は、多くのワークフローにとって重要な要素です。これまで、通知は個々の電子メール通知を使用して提供されていましたが、GDPR イベント通知は Adobe I/O イベントを活用するメッセージです。このメッセージは、設定済みの Webhook に送信されるので、ジョブリクエストの自動化が容易になります。[!DNL Privacy Service] UI ユーザーは、Adobe I/O GDPR イベントを登録することで、製品のジョブまたは GDPR ジョブが完了したときに更新を受け取ることができます。 |

## 2019年4月18日

### 強化された機能

* Default range for the status table in the [!DNL Privacy Service] UI modified to a 7-day span.
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

* Fixed an issue where customers could not load the [!DNL Privacy Service] UI.