---
keywords: Experience Platform;getting started;attribution ai;popular topics
solution: Experience Platform
title: Attribution AIの概要
topic: Getting started
translation-type: tm+mt
source-git-commit: 14d47f99f1edd7734245b25b7c39f3a71e7aac50

---


# Attribution AIの概要

以下のガイドでは、Attribution AIの使用に関連する様々なAdobe Experience Platformサービスについて理解している必要があります。 チュートリアルを開始する前に、次のドキュメントを確認します。

- [エクスペリエンスデータモデル(XDM)システム概要](../../xdm/home.md):XDMは、Experience Platformが提供するAdobe Experience Cloudが、適切なチャネルを適切なタイミングで適切な人に適切なメッセージを届けることを可能にする、基本的なフレームワークです。 Experience Platformが構築される方法、XDM Systemは、プラットフォームサービスで使用するエクスペリエンスデータモデルスキーマを運用します。
- [スキーマ構成の基本](../../xdm/schema/composition.md):このドキュメントでは、Experience Data Model(XDM)スキーマの概要と、Adobe Experience Platformで使用するスキーマを構成するための構成要素、原則およびベストプラクティスを紹介します。
- [スキーマ](../../xdm/tutorials/create-schema-ui.md):このチュートリアルでは、Experience Platform内でスキーマエディターを使用してスキーマを作成する手順を説明します。

アトリビューションAIでは、データセットが [Experience Data Model](../../xdm/home.md) (XDM)のミックスインであるConsumer Experience Data Model(CEE)スキーマに準拠している必要があります。 このデータを実装または変更するには、アドビサポート(attributionai-support@adobe.com)にお問い合わせください。 メディア支出データが存在する場合は、売上高の増加やROIの増加など、さらに分析を行うことができます。 顧客プロファイルデータが使用可能な場合、クレジットを顧客プロファイルレベルに属性付けできます。

## 用語

- **コンバージョンイベント:** 会議の登録など、目標に向けたマイルストーンを示すために顧客が行うデジタルイベントまたはデジタルインタラクション。 その他の例としては、有料コンバージョン、無料アカウントサインアップ、特性の資格などがあります。

- **タッチポイント：** 顧客が目標に向かうイベントの中で行うデジタルインタラクション。 例としては、購入前のマーケティング活動、表示された広告インプレッションの表示、有料検索クリックなどがあります。

## スコアへのアクセスとクエリ

>[!NOTE] 生のスコアのクエリやアクセスが不要な場合は、この手順をスキップし、ユーザーインターフェイスガイドに進 [むことができます](./user-guide.md)。

アトリビューションAIのスコアへのアクセスとクエリは、Snowflakeを通じて行われます。 現在、Snowflake用の秘密鍵証明書を設定して受け取るか、または生データを一括で書き出すには、attributionai-support@adobe.comでアドビサポートに電子メールでお問い合わせいただく必要があります。

アドビサポートがリクエストを処理すると、Snowflakeの読者アカウントのURLと、次の対応する資格情報が提供されます。

- 雪片URL
- ユーザー名
- パスワード

## 次の手順

準備が整い、すべての資格情報とスキーマが整ったら、 [Attribution AIユーザーインターフェイスガイドに従って開始します](./user-guide.md)。 このガイドでは、インスタンスの作成、およびトレーニングとスコアの送信に関する手順を説明します。