---
keywords: Experience Platform;ホーム;人気のトピック;ポリシーの適用;自動適用;API ベースの適用;データガバナンス
solution: Experience Platform
title: ポリシー適用の概要
topic-legacy: guide
description: データ使用ラベルが Adobe Experience Platform データセットに適用され、これらのラベルに対するマーケティングアクションに対してデータ使用ポリシーが定義されると、データガバナンス機能を使用して、これらのポリシーを適用し、ポリシーに違反するデータ操作を防ぐことができます。データガバナンス機能によって提供される Platform のポリシー適用には、API ベースの適用と自動適用の 2 つの方法があります。
exl-id: d19d8060-85a1-405c-856d-f59041947a33
source-git-commit: 03e7863f38b882a2fbf6ba0de1755e1924e8e228
workflow-type: ht
source-wordcount: '239'
ht-degree: 100%

---

# ポリシー適用の概要

データ使用ラベルがデータセットに適用され、これらのラベルに対するマーケティングアクションに対してデータ使用ポリシーが定義されると、Adobe Experience Platform データガバナンス機能を使用して、これらのポリシーを適用し、ポリシーに違反するデータ操作を防ぐことができます。

[!DNL Platform] のデータガバナンス機能によって提供されるポリシー施行には、API ベースの施行と自動施行の 2 つのメソッドがあります。

## API ベースの適用

[!DNL Policy Service] API は、マーケティングアクションをデータセットや任意のデータ使用ラベルの組み合わせに対してテストし、ポリシー違反が発生したかどうかを確認できるエンドポイントを提供します。その後、API 応答に基づいて、エクスペリエンスアプリケーション内でプロトコルを設定し、データ使用ポリシーのコンプライアンスを適切に実施できます。

API を使用してポリシーを評価する手順については、[API ベースの適用](./api-enforcement.md)に関するチュートリアルを参照してください。

## 自動適用

Experience Platform では、データ系列、データ分類、ポリシー管理機能を活用して、ポリシー違反を自動的に評価し、明らかにします。詳しくは、[自動ポリシー適用](./auto-enforcement.md)の概要を参照してください。
