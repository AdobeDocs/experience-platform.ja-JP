---
title: リソースの削除
description: Adobe Experience Platform でタグリソースを削除する方法について説明します。
exl-id: c8e26720-1976-48ec-8490-3d4ce587831e
source-git-commit: 8ded2aed32dffa4f0923fedac7baf798e68a9ec9
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 93%

---

# リソースの削除

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語が変更されました。用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

リソースを削除すると、そのリソースは Adobe Experience Platform から完全に削除されます。特定のタグライブラリからリソースを削除しても、そのリソースを他のライブラリで使用できるようにする場合は、[ ライブラリからのリソースの削除 ](remove-resources-from-library.md) に関するガイドを参照してください。

データ要素、ルール、拡張機能、ホスト、環境およびプロパティを削除できます。削除したリソースは復元できません。

ライブラリ（データ要素、ルール、拡張機能）に追加されたリソースには、削除時に特別な考慮事項があります。

## 削除するリソースの準備

リソースは異なる状態で存在し、相互に依存しています。リソースを削除する前に、削除できる状態にあることを確認する必要があります。

削除するリソースの準備は、次の 2 つの基本手順で構成されます。

1. 依存関係を解決する。
1. ライブラリから削除する。

### 依存関係を解決する

ルール、データ要素、拡張機能は互いに依存しているので、ほとんど場合、削除すると、カスケード効果が発生し、他にもクリーンアップが必要となります。

#### ルール

ルールは、他のリソース（拡張機能とデータ要素）に依存しますが、それらに依存するリソースはありません。ルールを削除すると、ライブラリで使用することも、表示することもできなくなりますが、その後クリーンアップする依存関係がなくなります。

#### データ要素

データ要素は拡張機能に依存しますが、ルールとは異なり、データ要素にはルールと、そのデータ要素に依存する拡張機能を含めることができます。データ要素を削除すると、このデータ要素に依存するルールや拡張機能は影響を受けます。

削除すると、データ要素は、実行時に正しい値を返さなくなります。空の文字列または %% で囲われた削除済みデータ要素の名前を返します（例：`%data-element-name%`）。この動作はプロパティ設定内で設定できます。

データ要素を削除する前後に、これらの依存関係を解決できます。

#### 拡張機能

その他すべてのリソース（ルール、ルールコンポーネントおよびデータ要素）は、拡張機能によって提供されます。

ルールコンポーネントとデータ要素は、動作の拡張機能によって異なりますが、データ収集ユーザーインターフェイスに表示されるだけです。依存関係を解決する前に拡張機能を削除すると、これらの孤立したリソースを表示できなくなります。これらの孤立したリソースはリスト表示に表示されますが、詳細ビューを開こうとするとエラーが表示されます。

このため、拡張機能を削除する際は細心の注意を払い、削除する前に依存関係を解決する必要があります。

### ライブラリから削除する

リソースを削除する前に、そのリソースを含むライブラリからリソースを削除する必要があります。このプロセスは、ライブラリの状態によって異なります。

#### 開発

1. ライブラリを開きます。
1. リソースを削除します。
1. ライブラリを保存します。
1. リソースを削除します。

#### 送信済みまたは承認済み

1. ライブラリを拒否します（開発版に戻します）。
1. 上記の手順に従って、リソースを開発用ライブラリから削除します。

#### 実稼動

1. リソースを無効にします。
1. 無効になっているリソースを実稼動環境に公開します。
1. リソースを削除します。

## リソースの削除

適切なリスト表示から、削除するリソースを選択し、「**[!UICONTROL 削除]**」を選択します。
