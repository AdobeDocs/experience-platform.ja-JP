---
description: Experience Platform がストリーミング宛先によって返される様々なタイプのエラーをどのように処理し、宛先プラットフォームへのデータ送信をどのように再試行するかを説明します。
title: Destination SDK で作成されたストリーミング宛先のレート制限および再試行ポリシー
exl-id: aad10039-9957-4e9e-a0b7-7bf65eb3eaa9
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 100%

---

# Destination SDK で作成されたストリーミング宛先のレート制限および再試行ポリシー

パートナーが作成した宛先は、様々なエラーを返し、異なるレート制限ポリシーを持つ可能性があります。このページでは、ストリーミング宛先によって返される様々なタイプのエラーを Experience Platform がどのように処理するかについて説明します。

Destination SDK を使用して宛先を設定する場合、[ベストエフォート集計](../functionality/destination-configuration/aggregation-policy.md#best-effort-aggregation)と[設定可能な集計](../functionality/destination-configuration/aggregation-policy.md#configurable-aggregation)の 2 つの集計タイプから選択できます。選択する集計タイプに応じて、以下の Experience Platform によるエラーおよびレート制限の処理方法を参照してください。

## ベストエフォート集計 {#best-effort-aggregation}

宛先に対する任意の HTTP 呼び出しに失敗した場合、Experience Platform は、最初の呼び出しの後、即座にもう一度、呼び出しを試します。2 回目の呼び出しでも失敗する場合、Experience Platform は、呼び出しをドロップして、3 回目の再試行は行いません。

## 設定可能な集計 {#configurable-aggregation}

設定可能な集計で設定された宛先プラットフォームの場合、Experience Platform は、プラットフォームによって返されるエラータイプを以下のように区別します。

* Experience Platform がプラットフォームへのデータ送信を再試行するエラー：
   * HTTP 応答コード 420 および 429
   * HTTP 応答コード 500 番台
* Experience Platform がプラットフォームへのデータ送信を再試行&#x200B;*しない*&#x200B;エラー：プラットフォームによって返されるその他のすべてのエラー

### 再試行アプローチについて {#retry-approach}

設定可能な集計に対する Experience Platform のアプローチを以下に示します。この例では、1 分間に 50,000 回を超えるリクエストを受信すると 429 エラーコードを返し始める宛先プラットフォームに Experience Platform がデータを送信することを想定しています。

* 1 分：Experience Platform がプロファイルを含む 40,000 件のバッチを集計して、宛先プラットフォームに送信します。Experience Platform は、40,000 件の HTTP リクエストを行い、すべて成功します。
* 2 分：Experience Platform がプロファイルを含む 70,000 件のバッチを集計して、宛先プラットフォームに送信します。Experience Platform は、70,000 件の HTTP リクエストを行い、50,000 件が成功します。その他の 20,000 件は、エンドポイントからレート制限エラーを受け取り、30 分後に再試行します。
* 3 分：Experience Platform がプロファイルを含む 30,000 件のバッチを集計して、宛先プラットフォームに送信します。Experience Platform は、30,000 件の HTTP リクエストを行い、すべて成功します。
* ...
* ...
* 32 分：Experience Platform は、2 分の時点で失敗していた 20,000 件のバッチの送信を再試行します。すべての呼び出しが成功します。

## 次の手順 {#next-steps}

ストリーミング宛先を設定した際に選択した集計ポリシーに応じて、Experience Platform が宛先プラットフォームからのエラーおよびレート制限をどのように処理するかについて確認しました。次に、以下のドキュメントを確認できます。

* [宛先設定のテスト](../testing-api/streaming-destinations/streaming-destination-testing-overview.md)
* [Destination SDK で作成した宛先をレビュー用に送信](../guides/submit-destination.md)
