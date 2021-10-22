---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: プライバシーに関するサービスリリースノート
topic-legacy: release notes
description: Adobe エクスペリエンス Platform Privacy サービスの最新リリースノート。
exl-id: 66ee38f1-f0d5-44ff-823d-d1b8a9765c6d
source-git-commit: 82dea48c732b3ddea957511c22f90bbd032ed9b7
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 58%

---

# [!DNL Privacy Service] リリースノート

このドキュメントでは、Adobe エクスペリエンスプラットフォーム用の新機能について説明すると [!DNL Privacy Service] 共に、機能強化および重要なバグ修正についても説明します。

>[!NOTE]
>
>その他のサービスについては、最新のリリースノートを [!DNL Experience Platform] 参照 [ して ](../release-notes/latest/latest.md) ください。

## 2020 年 9 月 9 日（PT）

### 新機能

| 機能 | 説明 |
| --- | --- |
| LGPD のサポート (ブラジル) | ブラジルの (LGPD) 法を使用して、プライバシーに関するジョブを作成できるようになりました [!DNL Lei Geral de Proteção de Dados] 。 このようなジョブは、規制コードによって追跡され `lgpd_bra` ます。 |

## 2020年4月8日

### 新機能

| 機能 | 説明 |
| --- | --- |
| PDPA のサポート | [!DNL Privacy] タイの Personal Data Protection 法 (PDPA) によって、要求を作成し、追跡することができるようになりました。 API でプライバシーリクエストを実行する場合、`regulation` 配列は「pdpa_tha」の値を受け取ります。 |
| UI の名前空間タイプ | UI で、要求ビルダーで別の名前空間タイプを指定できるようになりました [!DNL Privacy Service] 。 詳しくは、[ユーザガイド](ui/user-guide.md)を参照してください。 |
| 古いエンドポイントの廃止 | 古い API エンドポイント（`data/privacy/gdpr`）は非推奨となりました。 |

## 2020年1月14日

### 新機能

| 機能 | 説明 |
| --- | --- |
| [!DNL Privacy Service] rebranding | 以前は、 [!DNL Privacy Service] GDPR という他の規制をサポートするためにサービスが成長したので、以前の「GDPR サービス」が再ブランディングされていました。 |
| 新しい API エンドポイント | API のベースパス [!DNL Privacy Service] がに更新されました。 `/data/privacy/gdpr``/data/core/privacy/jobs` |
| 新しい必須の `regulation` プロパティ | API で新しいジョブを作成する場合は [!DNL Privacy Service] 、その `regulation` ジョブを追跡する規制を指定するために、要求ペイロードにプロパティを指定する必要があります。 指定できる値は、`gdpr` および `ccpa` です。詳しくは、「 [ ](api/privacy-jobs.md) API guide」の「プライバシーに関する作業」を参照してください [!DNL Privacy Service] 。 |
| Adobe Primetime Authentication のサポート | [!DNL Privacy Service] では、製品の値として `primetimeAuthentication` を使用して、Adobe Primetime Authentication からのアクセス / 削除リクエストを受け入れるようになりました。詳しくは、[Primetime 認証のドキュメント](http://tve.helpdocsonline.com/how-to-make-a-privacy-request)を参照してください。 |

### 強化された機能

* [!DNL Privacy Service] UI の機能強化:
   * GDPR 規制と CCPA 規制下でのジョブトラッキングに関するページが分離されました。
   * GDPR と CCPA 下でのトラッキングデータを切り替えるための「*規制の種類*」ドロップダウンが新しく追加されました。

## 2019年7月25日

### 新機能

| 機能 | 説明 |
| --- | --- |
| リクエスト指標ダッシュボード | UI の新しいメトリックスダッシュボードには、 [!DNL Privacy Service] 送信された GDPR 要求、状況によっては正常な各要求が表示されます。 |
| リクエストビルダー | サービス組織の技術ユーザーとそれ以外のユーザーの両方が GDPR リクエストを送信する場合、UI にリクエストを作成する機能が追加されます。JSON ファイル送信機能は、 [!DNL Privacy Service] 引き続き使用している組織の UI でも使用できます。 |
| GDPR ジョブイベントの通知 | GDPR ジョブのステータスに関するイベント通知は、多くのワークフローにとって重要な要素です。これまで、通知は個々の電子メール通知を使用して提供されていましたが、GDPR イベント通知は Adobe I/O イベントを活用するメッセージです。このメッセージは、設定済みの Webhook に送信されるので、ジョブリクエストの自動化が容易になります。[!DNL Privacy Service] UI ユーザーは、Adobe I/O GDPR イベントを登録することで、製品のジョブまたは GDPR ジョブが完了したときに更新を受け取ることができます。 |

## 2019年4月18日

### 強化された機能

* UI の状態テーブルの初期設定範囲が [!DNL Privacy Service] 7 日間に変更されました。
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

* ユーザーが UI を読み込めないという問題を修正しました [!DNL Privacy Service] 。
