---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Privacy Serviceのリリースノート
topic: release notes
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 5%

---


# Privacy Serviceのリリースノート

このドキュメントには、Adobe Experience Platform Privacy Serviceの新機能に関する情報と、拡張機能および重要なバグ修正が含まれています。

>[!NOTE]
>
>他のExperience Platformサービスの最新のリリースノートは、 [こちらを参照してください](../release-notes/latest/latest.md)。

## 2020 年 4 月 9 日

### 新機能

| 機能 | 説明 |
| --- | --- |
| PDPAのサポート | タイのPersonal Data Protection Act(PDPA)で、プライバシー要求を作成し、追跡できるようになりました。 APIでプライバシーリクエストを行う場合、 `regulation` 配列は値「pdpa_tha」を受け入れます。 |
| UIの名前空間タイプ | Privacy ServiceUIのリクエストビルダーで、異なる名前空間タイプを指定できるようになりました。 詳しくは、 [ユーザガイド](ui/user-guide.md) を参照してください。 |
| 古いエンドポイントの廃止 | 古いAPIエンドポイント(`data/privacy/gdpr`)は非推奨となりました。 |

## 2020 年 1 月 14 日

### 新機能

| 機能 | 説明 |
| --- | --- |
| Privacy Serviceリブランディング | GDPRに加え、他の規制をサポートするようになったため、従来の「GDPRサービス」はPrivacy Serviceにリブランドされました。 |
| 新しいAPIエンドポイント | Privacy ServiceAPIのベースパスが、からに更新され `/data/privacy/gdpr` ました `/data/core/privacy/jobs` |
| 新しい必須 `regulation` プロパティ | Privacy ServiceAPIで新しいジョブを作成する場合、要求ペイロードで `regulation` プロパティを指定して、ジョブを追跡する規則を示す必要があります。 指定できる値は `gdpr` とで `ccpa`す。 詳しくは、Privacy Service開発者ガイドの [プライバシージョブに関するドキュメント](api/privacy-jobs.md) （英語のみ）を参照してください。 |
| Adobe Primetime認証のサポート | Privacy Serviceは、製品の値としてを使用し、Adobe Primetime Authenticationからのアクセス/削除要求 `primetimeAuthentication` を受け入れるようになりました。 See the [Primetime Authentication documentation](http://tve.helpdocsonline.com/how-to-make-a-privacy-request) for more information. |

### 強化された機能

* Privacy ServiceUIの強化：
   * GDPRおよびCCPA規制に関する個別のジョブトラッキングページ。
   * GDPRとCCPAのトラッキングデータを切り替えるための新しい _Regulation Type_ （規則タイプ）ドロップダウン。

## 2019 年 7 月 25 日

### 新機能

| 機能 | 説明 |
| --- | --- |
| リクエスト指標ダッシュボード | Privacy ServiceUIの新しい指標ダッシュボードは、送信されたGDPRリクエスト、エラー・リクエスト、完了したGDPRリクエストを表示します。 |
| リクエストビルダー | GDPR要求を提出する技術的なユーザーと技術的でないユーザーの両方を持つ組織に対して、UIに「リクエストの作成」機能が追加されました。 JSONファイル送信機能は、引き続き使用したい組織のPrivacy ServiceUIでも使用できます。 |
| GDPRジョブイベント通知 | GDPRジョブのステータスに関するイベント通知は、多くのワークフローにとって重要な要素です。 以前は個々の電子メール通知を使用して通知が提供されていましたが、GDPRイベント通知はAdobe I/Oイベントを利用するメッセージで、設定済みのWebフックに送信され、ジョブリクエストの自動化が容易になります。 Privacy ServiceUIのユーザーは、Adobe I/O GDPRイベントに登録して、製品またはGDPRジョブが完了したときに更新を受け取ることができます。 |

## 2019 年 18 月 5 日

### 強化された機能

* Privacy ServiceUIのステータステーブルのデフォルトの範囲が7日間に変更されました。
* 内部例外処理の改善。
* データ変更率が低い一般的な内部呼び出しのキャッシュを導入することで、パフォーマンスが向上しました。

### バグの修正

* Privacy ServiceAPIのエンドポイントに、フィルターされたクエリに関する見つからない `GET /` ログ情報を追加しました。

## 2019 年 4 月 12 日

### 強化された機能

* ベータ版のお客様向けの新機能をサポートするUIを更新
* ベータ版のUI 2.0機能をサポートする新しい指標API

## 2019 年 4 月 10 日

### 強化された機能

* すべてのルックアップ(GET) API呼び出しを、デフォルトの30日のルックバック範囲に更新しました。
* APIの使用が制限され、最大45日のルックバック範囲を持つようになりました。

## 2019 年 14 月 3 日

### 強化された機能

* 各POST送信でこの `include` フィールドを強制します。
* JSONのアップロード時にこの `include` フィールドを強制的に設定します。

### バグの修正

* ユーザーがPrivacy ServiceUIを読み込めない問題を修正しました。