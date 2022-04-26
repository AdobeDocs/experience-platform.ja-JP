---
description: Experience Platformがストリーミング宛先で返される様々なタイプのエラーを処理する方法と、宛先プラットフォームへのデータの送信を再試行する方法について説明します。
title: Destination SDKで構築されたストリーミング宛先のレート制限および再試行ポリシー
source-git-commit: ec50608f6454dd619c30b6337f454561844183ba
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 4%

---

# Destination SDKで構築されたストリーミング宛先のレート制限および再試行ポリシー

## 概要 {#overview}

パートナーが構築した宛先は、様々なエラーを返し、異なるレート制限ポリシーを持つ場合があります。 このページでは、Experience Platformがストリーミング先から返された様々なタイプのエラーを処理する方法について説明します。

宛先を設定する際に、Destination SDKを使用して 2 つの集計タイプから選択できます。 [ベストエフォート集計](/help/destinations/destination-sdk/destination-configuration.md#best-effort-aggregation) および [設定可能な集計](/help/destinations/destination-sdk/destination-configuration.md#configurable-aggregation). 選択した集計の種類に応じて、次に示すように、Experience Platformがエラーとレートの制限を処理する方法を示します。

## ベストエフォート集計 {#best-effort-aggregation}

宛先に対する HTTP 呼び出しが失敗した場合、Experience Platformは、最初の呼び出しの直後に、もう一度呼び出しをおこなおうとします。 2 回目の試行でも呼び出しが失敗する場合、Experience Platformは呼び出しを破棄し、3 回目の再試行はおこないません。

## 構成可能な集計 {#configurable-aggregation}

設定可能な集計を使用して設定された宛先プラットフォームの場合、Experience Platformは、プラットフォームから返されるエラータイプを区別します。

* Experience Platformがプラットフォームへのデータの送信を再試行するエラー：
   * HTTP 応答コード 420 および 429
   * 500 を超える HTTP 応答コード
* Experience Platform *次の値と等しくない* データをプラットフォームに送信し直します。プラットフォームから返されるその他すべてのもの

### 再試行方法の説明 {#retry-approach}

設定可能な集計のExperience Platformアプローチを以下に示します。 この例では、Experience Platformが 1 分あたり 50,000 件を超えるリクエストを受信した場合、429 エラーコードを返し始める宛先プラットフォームにデータを送信すると仮定します。

* 分 1:Experience Platformは、40,000 個のバッチをプロファイルと共に集計して、宛先プラットフォームに送信します。 Experience Platformが 40,000 個の HTTP リクエストをおこない、すべてが成功しました。
* 分 2:Experience Platformは、70,000 個のバッチをプロファイルと共に集計して、宛先プラットフォームに送信します。 Experience Platformが 70,000 個の HTTP リクエストを実行し、50,000 個が成功しました。 残りの 20k は、エンドポイントからレート制限エラーを受け取り、30 分後に再試行されます。
* 分 3:Experience Platformは、30,000 個のバッチをプロファイルと共に集計して、宛先プラットフォームに送信します。 Experience Platformが 30,000 個の HTTP リクエストを実行し、すべてが成功しました。
* ...
* ...
* 分 32:Experience Platformは、2 分目に失敗した 20,000 個のバッチの送信を再試行します。 すべての呼び出しが成功しました。

## 次の手順 {#next-steps}

これで、Experience Platformが宛先プラットフォームでエラーとレート制限を処理する方法がわかりました。これは、ストリーミング宛先の設定時に選択した集計ポリシーに応じて異なります。 次に、次のドキュメントを確認できます。

* [宛先設定のテスト](/help/destinations/destination-sdk/test-destination.md)
* [Destination SDK で作成した宛先のレビュー用に送信する](/help/destinations/destination-sdk/submit-destination.md)
