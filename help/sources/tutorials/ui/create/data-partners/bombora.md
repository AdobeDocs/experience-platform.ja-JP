---
title: UI を使用して Bombora の目的をExperience Platformに接続する
description: Bombora Intent をExperience Platformに結び付ける方法を学ぶ
hide: true
hidefromtoc: true
exl-id: 76a4fed5-b2d5-46d5-9245-b52792a7d323
source-git-commit: 911aad600dd2618ba98d2ccee737aaedea4f2735
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 53%

---

# UI を使用した [!DNL Bombora Intent] のExperience Platformへの接続

ユーザーインターフェイスを使用して [!DNL Bombora Intent] アカウントをAdobe Experience Platformに接続する方法については、このガイドを参照してください。

## 基本を学ぶ

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [Real-Time CDP B2B edition](../../../../../rtcdp/b2b-overview.md): Real-Time CDP B2B editionは、B2B サービスモデルで業務を行うマーケター向けに設計されています。 複数のソースからのデータをまとめて、人物とアカウントプロファイルの単一のビューに結合します。この統合されたデータにより、マーケターは特定のオーディエンスを正確にターゲットにして、利用可能なすべてのチャネルでそれらのオーディエンスを惹き付けることができます。
* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## ソースカタログのナビゲート

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

*[!UICONTROL B2B]* カテゴリの下の「**[!DNL Bombora Intent]**」を選択し、「**[!UICONTROL 設定]**」を選択します。

>[!TIP]
>
>ソースカタログ内のソースは、特定のソースがまだ認証済みのアカウントを持っていない場合に「**[!UICONTROL 設定]**」オプションを表示します。 認証済みアカウントが存在すると、このオプションは **[!UICONTROL データを追加]** に変わります。



## 既存のアカウントを使用 {#existing}

## 新しいアカウントを作成 {#create}

## データフローの詳細を入力 {#provide-dataflow-details}

>[!CONTEXTUALHELP]
>id="platform_sources_bombora_domain"
>title="ドメインソース"
>abstract="アドビでは XDM accountOrganization.website を使用していますが、各 Web サイトにカスタムフィールドを使用しているお客様が存在する場合があります。したがって、ドメインソースが Bombora アカウントレコードを Experience Platform アカウントと一致させるドメイン／web サイトフィールドであることを確認する必要があります。"

## データフローのスケジュール {#schedule-dataflow}

>[!CONTEXTUALHELP]
>id="platform_sources_bombora_schedule"
>title="データフローのスケジュール"
>abstract="Bombora は、毎週月曜日の午後 5:00（UTC）にデータをドロップします。したがって、取り込み開始時刻を午後 5:00（UTC）以降に設定する必要があります。また、アドビにファイルをドロップする際にスケジュールを変更する場合があるので、取り込み時間を Bombora に確認する必要があります。"


## データフローのレビュー {#review-dataflow}
