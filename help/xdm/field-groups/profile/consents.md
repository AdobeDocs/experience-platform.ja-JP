---
solution: Experience Platform
title: 同意および環境設定スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「同意」および「環境設定」スキーマフィールドグループの概要を説明します。
exl-id: ec592102-a9d3-4cac-8b94-58296a138573
source-git-commit: 6caece867afe3e6f3fd323843b753cce2319623c
workflow-type: tm+mt
source-wordcount: '941'
ht-degree: 0%

---

# [!UICONTROL 同意および環境設定] フィールドグループ

[!UICONTROL 同意および環境設定] は、 [[!DNL XDM Individual Profile] クラス](../../classes/individual-profile.md) 。個々の顧客の同意および環境設定情報を取り込みます。

>[!NOTE]
>
>このフィールドグループは [!DNL XDM Individual Profile]の場合、 [!DNL XDM ExperienceEvent] スキーマ。 エクスペリエンスイベントスキーマに同意データと環境設定データを含める場合は、 [[!UICONTROL プライバシー、パーソナライゼーションおよびマーケティング環境設定に対する同意] データタイプ](../../data-types/consents.md) を使用してスキーマに追加する [カスタムフィールドグループ](../../ui/resources/field-groups.md#create) 代わりに、

## フィールドグループ構造 {#structure}

この [!UICONTROL 同意および環境設定] フィールドグループには、単一のオブジェクトタイプフィールドが用意されています。 `consents`：同意および環境設定の情報を取り込みます。 このフィールドは、 [[!UICONTROL プライバシー、パーソナライゼーションおよびマーケティング環境設定に対する同意] データタイプ](../../data-types/consents.md)、 `adID` フィールドに値を入力し、 `idSpecific` マップフィールド。

![](../../images/field-groups/consent.png)

>[!TIP]
>
>詳しくは、 [XDM リソースの調査](../../ui/explore.md) を参照して、XDM リソースを検索し、Platform UI でその構造を調べる手順を確認してください。

次の JSON は、 [!UICONTROL 同意および環境設定] フィールドグループを処理できます。 フィールドグループが提供するほとんどのフィールドの使用方法について詳しくは、 [同意および環境設定データタイプ](../../data-types/consents.md). 以下のサブセクションでは、フィールドグループがデータ型に追加する一意の属性に焦点を当てます。

```json
{
  "consents": {
    "collect": {
      "val": "VI"
    },
    "share": {
      "val": "y"
    },
    "personalize": {
      "content": {
        "val": "y"
      }
    },
    "marketing": {
      "preferred": "email",
      "any": {
        "val": "y"
      },
      "email": {
        "val": "y"
      }
    },
    "idSpecific": {
      "ECID": {
        "37784337855396895622558625508046772577": {
          "adID": {
            "val": "n",
          },
          "share": {
            "val": "n"
          },
          "marketing": {
            "push": {
              "val": "n",
              "time": "2020-09-30T01:02:33+00:00",
              "reason": "not relevant"
            }
          }
        }
      },
      "email": {
        "john@xyz.com": {
          "marketing": {
            "email": {
              "val": "y"
            }
          }
        }
      }
    },
    "metadata": {
      "time": "2019-01-01T15:52:25+00:00"
    }
  }
}
```

>[!TIP]
>
>顧客の同意と環境設定データのマッピング方法を視覚化するのに役立つ、Experience Platformで定義した任意の XDM スキーマのサンプル JSON データを生成できます。 詳しくは、次のドキュメントを参照してください。
>
>* [UI でのサンプルデータの生成](../../ui/sample.md)
>* [API でサンプルデータを生成する](../../api/sample-data.md)


### `idSpecific`

`idSpecific` は、特定の同意や環境設定がお客様に一般に適用されるわけではなく、1 つのデバイスまたは ID に制限される場合に使用できます。 例えば、顧客はあるアドレス宛ての E メールの受信をオプトアウトし、別のアドレス宛ての E メールを許可することができます。

>[!IMPORTANT]
>
>チャネルレベルの同意および環境設定 ( `consents` 外 `idSpecific`) は、そのチャネル内のすべての ID に適用されます。 したがって、すべてのチャネルレベルの同意および環境設定は、同等の ID 設定とデバイス固有の設定のどちらが適用されるかに直接影響します。
>
>* 顧客がチャネルレベルでオプトアウトした場合、 `idSpecific` は無視されます。
>* チャネルレベルの同意または環境設定が設定されていない場合、または顧客がオプトインした場合、 `idSpecific` は光栄です。


各キー `idSpecific` オブジェクトは、Adobe Experience Platform ID サービスで認識される特定の ID 名前空間を表します。 独自のカスタム名前空間を定義して異なる識別子を分類できますが、ID サービスが提供する標準名前空間の 1 つを使用して、リアルタイム顧客プロファイルのストレージサイズを減らすことをお勧めします。 ID 名前空間について詳しくは、 [id 名前空間の概要](../../../identity-service/namespaces.md) （ ID サービスドキュメント）を参照してください。

各 namespace オブジェクトのキーは、顧客が設定した一意の ID 値を表します。 各 ID 値には、 `consents`.

```json
"idSpecific": {
  "email": {
    "jdoe@example.com": {
      "marketing": {
        "email": {
          "val": "n"
        }
      }
    }
  },
  "ECID" : {
    "37784337855396895622558625508046772577": {
      "collect": {
        "val": "y"
      },
      "adID": {
        "val": "n"
      },
      "marketing": {
        "push": {
          "val": "n"
        }
      }
    }
  }
}
```

内 `marketing` 指定されたオブジェクト `idSpecific` セクション、 `any` および `preferred` フィールドはサポートされていません。 これらのフィールドは、ユーザーレベルでのみ設定できます。 また、 `idSpecific` マーケティング環境設定 `email`, `sms`、および `push` サポートしない `subscriptions` フィールド。

また、 `idSpecific` セクション： `adID`. このフィールドについては、以下のサブセクションで説明します。

#### `adID`

この `adID` 同意とは、広告主 ID（IDFA または GAID）を使用してこのデバイス上のアプリ間で顧客をリンクできるかどうかに関する顧客の同意を表します。 この値は、 `ECID` ID 名前空間 ( `idSpecific` 」セクションに追加し、他の名前空間やこのフィールドグループのユーザーレベルで設定することはできません。

```json
"idSpecific": {
  "ECID" : {
    "37784337855396895622558625508046772577": {
      "collect": {
        "val": "y"
      },
      "adID": {
        "val": "n"
      },
      "marketing": {
        "push": {
          "val": "n"
        }
      }
    }
  }
}
```

>[!NOTE]
>
>Adobe Experience Platform Mobile SDK は、必要に応じて自動的に設定するので、この値を直接設定する必要はありません。

## フィールドグループを使用したデータの取り込み {#ingest}

を使用するには、 [!UICONTROL 同意および環境設定] フィールドグループを使用して、お客様の同意データを取り込むには、そのフィールドグループを含むスキーマに基づいたデータセットを作成する必要があります。

に関するチュートリアルを参照してください。 [UI でのスキーマの作成](http://www.adobe.com/go/xdm-schema-editor-tutorial-en) フィールドグループをフィールドに割り当てる手順については、を参照してください。 以下の手順で、 [!UICONTROL 同意および環境設定] フィールドグループ ( [データセットの作成](../../../catalog/datasets/user-guide.md#create) データセットユーザーガイドの手順に従って、既存のスキーマを使用してデータセットを作成します。

>[!IMPORTANT]
>
>に同意データを送信する場合 [!DNL Real-time Customer Profile]の場合、 [!DNL Profile]に基づいて有効化されるスキーマ [!DNL XDM Individual Profile] を含むクラス [!UICONTROL 同意および環境設定] フィールドグループを使用します。 そのスキーマに基づいて作成するデータセットも有効にする必要があります [!DNL Profile]. 以下に関連する具体的な手順については、上記にリンクされているチュートリアルを参照してください。 [!DNL Real-time Customer Profile] スキーマとデータセットの要件。
>
>また、顧客プロファイルを正しく更新するために、最新の同意データと環境設定データを含むデータセットを優先するように結合ポリシーが設定されていることを確認する必要があります。 概要については、 [結合ポリシー](../../../rtcdp/profile/merge-policies.md) を参照してください。

## 同意と環境設定の変更の処理

顧客が Web サイトでの同意や環境設定を変更した場合、その変更は [Adobe Experience Platform Web SDK](../../../edge/consent/supporting-consent.md). 顧客がデータ収集をオプトアウトした場合、すべてのデータ収集は直ちに停止する必要があります。 顧客がパーソナライゼーションをオプトアウトした場合、訪問する次のページにパーソナライゼーションが存在しないはずです。

## 次の手順

このドキュメントでは、 [!UICONTROL 同意および環境設定] フィールドグループを使用します。 フィールドグループで提供されるその他のフィールドの詳細については、 [[!UICONTROL プライバシー、パーソナライゼーションおよびマーケティング環境設定に対する同意] データタイプ](../../data-types/consents.md).
