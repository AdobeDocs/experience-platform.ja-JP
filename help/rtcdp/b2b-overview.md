---
keywords: RTCDP;CDP;B2B Edition;Real-time Customer Data Platform;リアルタイムの顧客データプラットフォーム;リアルタイム cdp;b2b;cdp;顧客 AI
title: Real-time CDP B2B Edition の概要
description: Real-time Customer Data Platform B2B Edition アカウントの概要
exl-id: 9b45bba4-fc46-4d69-b36a-5cb91f316612
source-git-commit: ad0d38cbd249642d582a807c5679065827f57717
workflow-type: tm+mt
source-wordcount: '1083'
ht-degree: 100%

---

# Real-time Customer Data Platform B2B Edition の概要

Real-time Customer Data Platform（Real-time CDP）上に構築された Real-time CDP B2B Edition は、B2B サービスを行っているマーケター向けに設計されています。複数のソースからのデータをまとめて、人物とアカウントプロファイルの単一のビューに結合します。この統合されたデータにより、マーケターは特定のオーディエンスを正確にターゲットにして、利用可能なすべてのチャネルでそれらのオーディエンスを惹き付けることができます。

Real-time CDP B2B Edition と対応する B2C を区別する様々な Adobe Experience Platform 機能が改善されています。これには、B2B のユースケースに対するエクスペリエンスデータモデル（XDM）の改善、ID 解決とプロファイルセグメント化のアップグレード、[!DNL Marketo Engage] のカスタムビルドのコネクタと宛先が含まれます。[!DNL Marketo] コネクタを使用すると、B2B ブランドは、業界をリードする B2B エンゲージメントデータを行動情報と結び付けて、リードを育成し、アカウントベースのマーケティングオペレーションを強化できます。

Real-time CDP B2B Edition を使用すると、次のことができるようになります。

* 複数のソースから収集されたデータを 1 つのビューに結合して、全体的な人物とアカウントのプロファイルを作成する
* 統合されたアカウントプロファイルの一元化された保存データから、すべてのクロスソースデータを強化、セグメント化、エクスポートする
* 一元化プロセスのすべてのステップで利用できるデータガバナンスツールを使用してデータを管理し、データが法規制やビジネスポリシーに準拠していることを確認する

Real-time CDP B2B Edition で行われた改善に関するより包括的な詳細は、以下の節に分かれています。

## XDM

Real-time CDP B2B Edition は、B2B の目的のためにデータをキャプチャして構造化するために、複数の新しい XDM スキーマクラス、フィールドグループ、関係タイプを提供します。これらの各機能強化の分類については、[Real-time CDP B2B Edition の XDM](./schemas/b2b.md)の概要を参照してください。

事前構成された B2B スキーマを使用することにより、標準化された実用的な構造でデータを取り込むことができます。新しいスキーマクラスの多くは、[!DNL Salesforce]、[!DNL Microsoft Dynamics]、[!DNL Marketo]、その他の B2B データソースなど、主流の CRM で発生するものにほぼ直接マップされます。Real-time CDP B2B Edition を使用すると、B2B ソースからのデータをわかりやすい方法でプラットフォームに取り込むことができ、結果を簡単に監査できます。

これらの XDM の機能強化により、B2B 中心のソースと宛先を介してデータをより適切に取り込み、アクティベートできるため、データの統合と表示が改善され、より多様で柔軟なユースケースに対応できます。

## ID 解決

スキーマが定義され、それらのスキーマに従ってデータが取り込まれた後、Real-time CDP B2B Edition は、強力なリアルタイム ID 解決システムを通じて、現実の人物や企業を表すソースレコードを識別します。

ID 解決システムは、次の機能を提供します。

* B2B と B2C の人物の記録の組み合わせ
* マルチレベルのアカウント階層
* 多対多、人物からアカウントへの接続
* 人物とアカウントの ID はリアルタイムで解決

ID 解決システムは、より多面的な人物の分類をサポートするように拡張されました。このシステムにより、顧客だけでなくビジネスチャンスのリードとして人物を特定することができます。

ソース CRM によって同期され、システム内の複数のパスを介して接続されたアカウントレコードは、プラットフォームによって結合されます。このシステムは、ビジネスチャンスに関連する人物と顧客として記録された人物を結び付けますが、識別可能な場合は、それらの区別を属性として保持することもできます。

一致する識別子は、複数のシステム間からのアカウントレコードをリンクして結合するために使用されます。アカウント階層は、このプロセス全体を通じて保持されます。差別化要因は、ユーザーがアカウントに関連付けられているかどうかを精査し、必要に応じてアカウントからそれらを分離する機能を提供するために使用されます。

## プロファイルとセグメント化

Real-time CDP B2B Edition がデータを取り込んで、人物、会社、属性および行動に関連する ID を解決すると、そのデータを使用してプロファイルが作成されます。次に、これらのプロファイルを閲覧可能なオーディエンスにセグメント化して、さまざまな宛先にアクティベートできます。

正しく実装されると、システムはメールアドレスなどの変更される可能性のある属性ではなく、一意のプライマリ識別子を使用して人物を追跡します。これは、ユーザーが仕事を変えても、システムはそれをフォローしていることを意味します。そのユーザーはまだ同じエンティティですが、代わりに新しいアカウントにリンクされています。このネイティブ機能は、システムがすべての属性と動作を含む個人としてこれらの人物を追跡するため、新しいアカウントに拡張するための優れたベクトルを提供します。

## B2B ソース

Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。[!DNL Marketo] ソースを使用すると、B2B データを Platform にストリーミングし、Platform に接続されたアプリケーションを使用してこのデータを最新の状態に保つことができます。 [!DNL Marketo] のインスタンスをいくつでもサポートし（これは、複数のインスタンスを持つ大企業にとって有益です）、データが結合される単一の IMS 組織に引き込みます。

>[!NOTE]
>
>[!DNL Marketo] ソースは Real-time CDP B2B Edition を使用するために&#x200B;**必要ではありません**。

Marketo および B2B データを Platform に取り込む方法の詳細については、[Real-time CDP B2B Edition のドキュメントのソース](./sources/b2b.md)を参照してください。

## B2B の宛先

Google Customer Match、Facebook、LinkedIn、Marketo Engage、Amazon S3、Google Display &amp; Video 360、Google Ads および Google Ad Manager などの Experience Platform の宛先は、Real-time CDP B2B Edition で使用でき、完全にサポートされます。また、Platform からセグメントメンバーシップデータをストリーミングし、Marketo でリストとして利用できるようにする Marketo Engage の宛先もあります。

詳しくは、[Marketo Engageの宛先](../destinations/catalog/adobe/marketo-engage.md)の概要を参照してください。

複数の CRM を使用している企業の場合、Real-time CDP B2B エディションには、Marketo または CRM のインスタンスを分離するように宛先コネクタを設定するオプションがあります。必要に応じて、各インスタンスへの宛先コネクタを設定し、オーディエンスを各 CRM インスタンスに個別に送信できます。

## 次の手順

Real-time CDP B2B Edition が提供するマーケターのメリットと、Real-time CDP との違いについて理解が深まったので、これらの機能を独自の IMS 組織に適用する方法について学習できます。

Real-time CDP B2B Edition が B2B サービスモデルに与えるメリットについては、使用を開始するのに役立つ以下のドキュメントを参照してください。

* [Real-time CDP B2B Edition のユースケース](./b2b-use-case.md)
* [Real-time Customer Data Platform B2B Edition のエンドツーエンドのチュートリアル](./b2b-tutorial.md)
* [データの取り込み方法](./sources/b2b.md)
* [プロファイルへのアクセス方法](./profile/profile-overview.md)
* [Real-time Customer Data Platform B2B Edition のスキーマ](./schemas/b2b.md)
* [セグメントの作成方法](./segmentation/b2b.md)
* [宛先へのセグメントのアクティブ化の方法](./destinations/b2b.md)
* [データガバナンスポリシーの定義と実施方法](./privacy/data-governance-overview.md)
