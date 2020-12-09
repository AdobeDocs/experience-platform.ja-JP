---
keywords: Experience Platform;home;popular topics;api;API;XDM;XDM system;;experience data model;Experience data model;Experience Data Model;data model;Data Model;schema registry;Schema Registry;
solution: Experience Platform
title: Schema Registry API 開発者ガイド
description: 'スキーマレジストリAPIを使用すると、Experience Platform内で利用可能なすべてのスキーマと関連するXDMリソースをプログラムで管理できます。 '
topic: developer guide
translation-type: tm+mt
source-git-commit: 33f9ee45e8dd649d23f9b3b4f03ecf00d8e18fd2
workflow-type: tm+mt
source-wordcount: '961'
ht-degree: 5%

---


# [!DNL Schema Registry] API開発者ガイド

The [!DNL Schema Registry] is used to access the Schema Library within Adobe Experience Platform, providing a user interface and RESTful API from which all available library resources are accessible.

スキーマレジストリAPIは、複数のエンドポイントを提供します。エンドポイントを使用すると、Platform内で利用可能なすべてのスキーマおよび関連するExperience Data Model(XDM)リソースをプログラムで管理できます。 This includes those defined by Adobe, [!DNL Experience Platform] partners, and vendors whose applications you use.

これらのエンドポイントの概要を以下に示します。 詳しくは、個々のエンドポイントのガイドを参照してください。必要なヘッダーに関する重要な情報、サンプルAPI呼び出しの読み方などについては、 [はじめに](./getting-started.md) 「はじめに」のガイドを参照してください。

>[!IMPORTANT]
>
>XDMは、JSONスキーマ形式を使用して、取り込むカスタマーエクスペリエンスデータの構造を記述し、検証します。 スキーマレジストリAPIを使用する前に、この基盤となるテクノロジーの理解を深めるために、 [公式のJSONスキーマドキュメント](https://json-schema.org/) を確認することを強くお勧めします。

使用可能なすべてのエンドポイントとCRUD操作を表示するには、 [スキーマレジストリAPIリファレンスを参照してください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml)。

## スキーマ

XDMスキーマは、プラットフォームに取り込まれるデータの構造と形式を表し、検証します。 スキーマは、クラスと0個以上のミックスインで構成されます。 You can create, view, edit, and delete schemas using the `/schemas` endpoint. このエンドポイントの使用方法について詳しくは、『 [スキーマエンドポイントガイド](./schemas.md)』を参照してください。

ミックスインとデータ型の作成と追加を含む、スキーマレジストリAPIで完全なスキーマを作成する手順のガイドについては、 [APIスキーマの作成チュートリアルを参照してください](../tutorials/create-schema-api.md)。

## 動作

動作は、スキーマが記述するデータの特性を定義します。 各XDMクラスは、特定の動作を参照する必要があります。この動作は、そのクラスを使用するすべてのスキーマが継承します。 APIで使用可能な動作を表示する方法については、 [動作エンドポイントガイド](./behaviors.md) を参照してください。

## クラス

クラスは、そのクラスに基づくすべてのスキーマが含める必要がある共通プロパティの基本構造を定義し、これらのスキーマで使用できるミックスインを決定します。 すべてのクラスは、既存の動作に関連付ける必要があります。 APIでのクラスの操作について詳しくは、 [クラスエンドポイントガイド](./classes.md) を参照してください。

## ミックスイン

ミックスインは、個人、住所、Webブラウザー環境など、特定の概念を表す1つ以上のフィールドを定義する再利用可能なコンポーネントです。 ミックスインは、表すデータの動作（レコードまたは時系列）に応じて、互換性のあるクラスを実装するスキーマの一部として含めることを意図しています。 APIでのミッ [クスインの使用方法については、](./mixins.md) ミックスインエンドポイントガイドを参照してください。

## データタイプ

データ型は、基本的なリテラルフィールドと同様に、クラスまたはミックスインで参照型フィールドとして使用されます。重要な違いは、データ型で複数のサブフィールドを定義できる点です。 マルチフィールド構造の一貫した使用を可能にするというミックスインと同様ですが、ミックスインはルートレベルでのみ追加できるのに対し、スキーマ構造のどこにでも含めることができるので、データ型の柔軟性が向上します。 APIでのデータタイプの操作について詳しくは、 [データタイプエンドポイントガイド](./data-types.md) を参照してください。

## 記述子

記述子 は、スキーマ内の特定のフィールドに割り当てられた一連のメタデータで、これらのフィールド(およびスキーマ自体)が他のスキーマとどのように関連付けられているかなど、様々なコンテキストの詳細を提供します。 各スキーマは、1つ以上の記述子エンティティを適用でき、異なる目的を果たすための異なる種類の記述子を持ちます。 APIでの記述子の操作について詳しくは、 [記述子エンドポイントガイド](./descriptors.md) 、および異なる記述子の種類とその使用例の概要を参照してください。

## 和集合

Platformでは、特定の使用例に対してスキーマを作成できますが、特定のクラスに属するスキーマの「和集合」を作成することもできます。 和集合スキーマは、同じクラスを共有するすべてのスキーマのフィールドを1つの表現に集計します。 スキーマを [リアルタイム顧客プロファイルで使用できるようにすると](../../profile/home.md)、そのスキーマは特定のクラスの和集合に含まれます。 したがって、和集合スキーマを直接編集することはできず、プロファイルで使用するスキーマを含めたり除外したりする場合にのみ影響を受ける可能性があります。

スキーマレジストリAPIで和集合を表示する方法については、『 [和集合エンドポイントガイド](./unions.md)』を参照してください。

## 書き出し/読み込み

スキーマレジストリAPIを使用すると、サンドボックスとIMS組織間でXDMリソースを転送および共有できます。 任意のスキーマ、ミックスイン、またはデータタイプに対して、リソースの構造と依存するリソースを含む書き出しペイロードを生成できます。 その後、このペイロードを使用して、リソースを宛先サンドボックスおよびIMS組織にインポートできます。

このエンドポイントの使用について詳しくは、 [スキーマレジストリAPIリファレンス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml) を参照してください。

## サンプルデータ

スキーマライブラリ内の指定したスキーマのサンプルデータを生成できます。 返された応答オブジェクトは、データ取り込みのソースとして使用できます。

このエンドポイントの使用について詳しくは、 [スキーマレジストリAPIリファレンス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml) を参照してください。

## 監査ログ

スキーマレジストリは、異なる更新間でリソース(クラス、ミックスイン、データ型、またはスキーマ)に対して発生したすべての変更のログを保持します。 特定のリソースのログを取得するには、そのリソースを指定します。または、このエンドポイント `$id` へのGET要求のパス `meta:altId` を指定します。

このエンドポイントの使用について詳しくは、 [スキーマレジストリAPIリファレンス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml) を参照してください。

## 次の手順

スキーマレジストリAPIを使用した呼び出しを開始するには、は [じめにガイドを読み](./getting-started.md) 、特定のエンドポイントの使用方法を学ぶためのエンドポイントガイドの1つを選択します。