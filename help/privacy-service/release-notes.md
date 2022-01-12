---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Privacy Serviceリリースノート
topic-legacy: release notes
description: Adobe Experience Platform Privacy Serviceの最新のリリースノートです。
exl-id: 66ee38f1-f0d5-44ff-823d-d1b8a9765c6d
source-git-commit: dc81da58594fac4ce304f9d030f2106f0c3de271
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 58%

---

# [!DNL Privacy Service] リリースノート

このドキュメントでは、Adobe Experience Platformの新機能について説明します [!DNL Privacy Service]、機能強化および重要なバグ修正に加えて、

>[!NOTE]
>
>その他の最新のリリースノート [!DNL Experience Platform] サービスが見つかります [ここ](../release-notes/latest/latest.md).

## 2020 年 9 月 9 日（PT）

### 新機能

| 機能 | 説明 |
| --- | --- |
| LGPD のサポート（ブラジル） | ブラジルの [!DNL Lei Geral de Proteção de Dados] (LGPD) 規制。 これらのジョブは、規制コードの下で追跡されます `lgpd_bra`. |

## 2020年4月8日

### 新機能

| 機能 | 説明 |
| --- | --- |
| PDPA のサポート | [!DNL Privacy] タイの個人データ保護法 (PDPA) に基づいて、リクエストを作成し、トラッキングできるようになりました。 API でプライバシーリクエストを実行する場合、`regulation` 配列は「pdpa_tha」の値を受け取ります。 |
| UI の名前空間タイプ | リクエストビルダーで、異なるタイプの名前空間を [!DNL Privacy Service] UI 詳しくは、[ユーザガイド](ui/user-guide.md)を参照してください。 |
| 古いエンドポイントの廃止 | 古い API エンドポイント（`data/privacy/gdpr`）は非推奨となりました。 |

## 2020年1月14日

### 新機能

| 機能 | 説明 |
| --- | --- |
| [!DNL Privacy Service] 再ブランディング | 以前の名称「GDPR サービス」は、 [!DNL Privacy Service] は、GDPR に加えて他の規制もサポートするようになったためです。 |
| 新しい API エンドポイント | のベースパス [!DNL Privacy Service] API は次のように更新されました： `/data/privacy/gdpr` から `/data/core/privacy/jobs` |
| 新しい必須の `regulation` プロパティ | で新しいジョブを作成する場合 [!DNL Privacy Service] API, a `regulation` プロパティをリクエストペイロードで指定して、ジョブを追跡する規則を示す必要があります。 指定できる値は、`gdpr` および `ccpa` です。次のドキュメントを参照してください： [プライバシージョブ](api/privacy-jobs.md) 内 [!DNL Privacy Service] API ガイドを参照してください。 |
| Adobe Primetime Authentication のサポート | [!DNL Privacy Service] では、製品の値として `primetimeAuthentication` を使用して、Adobe Primetime Authentication からのアクセス / 削除リクエストを受け入れるようになりました。詳しくは、[Primetime 認証のドキュメント](https://tve.helpdocsonline.com/how-to-make-a-privacy-request)を参照してください。 |

### 強化された機能

* [!DNL Privacy Service] UI の強化：
   * GDPR 規制と CCPA 規制下でのジョブトラッキングに関するページが分離されました。
   * GDPR と CCPA 下でのトラッキングデータを切り替えるための「*規制の種類*」ドロップダウンが新しく追加されました。

## 2019年7月25日

### 新機能

| 機能 | 説明 |
| --- | --- |
| リクエスト指標ダッシュボード | 新しい指標ダッシュボード ( [!DNL Privacy Service] UI では、送信された GDPR リクエスト、エラーが発生した GDPR リクエスト、完了した GDPR リクエストを表示できます。 |
| リクエストビルダー | サービス組織の技術ユーザーとそれ以外のユーザーの両方が GDPR リクエストを送信する場合、UI にリクエストを作成する機能が追加されます。JSON ファイル送信機能は、引き続き [!DNL Privacy Service] 引き続き使用する組織の UI。 |
| GDPR ジョブイベントの通知 | GDPR ジョブのステータスに関するイベント通知は、多くのワークフローにとって重要な要素です。これまで、通知は個々の電子メール通知を使用して提供されていましたが、GDPR イベント通知は Adobe I/O イベントを活用するメッセージです。このメッセージは、設定済みの Webhook に送信されるので、ジョブリクエストの自動化が容易になります。[!DNL Privacy Service] UI ユーザーは、Adobe I/O GDPR イベントを登録することで、製品のジョブまたは GDPR ジョブが完了したときに更新を受け取ることができます。 |

## 2019年4月18日

### 強化された機能

* のステータステーブルのデフォルトの範囲 [!DNL Privacy Service] UI が 7 日間に変更されました。
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

* 顧客が [!DNL Privacy Service] UI
