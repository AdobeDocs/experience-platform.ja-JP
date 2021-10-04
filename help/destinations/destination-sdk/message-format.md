---
description: このページのコンテンツを、パートナーの宛先の残りの設定オプションと共に使用します。 このページでは、Adobe Experience Platformから宛先に書き出されるデータのメッセージ形式について説明し、他のページでは、宛先への接続と認証に関する詳細について説明します。
seo-description: Use the content on this page together with the rest of the configuration options for partner destinations. This page addresses the messaging format of data exported from Adobe Experience Platform to destinations, while the other page addresses specifics about connecting and authenticating to your destination.
seo-title: Message format
title: メッセージのフォーマット
exl-id: 1212c1d0-0ada-4ab8-be64-1c62a1158483
source-git-commit: a1e77520ba5555db42578eac261e01e77130aea2
workflow-type: tm+mt
source-wordcount: '2090'
ht-degree: 2%

---

# メッセージのフォーマット

## 前提条件 — Adobe Experience Platformの概念 {#prerequisites}

Adobe側のプロセスを理解するには、次のExperience Platform概念を理解してください。

* **エクスペリエンスデータモデル (XDM)**&#x200B;を参照してください。[XDM の概](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=ja) 要と  [Adobe Experience Platformでの XDM スキーマの作成方法](https://experienceleague.adobe.com/docs/experience-platform/xdm/tutorials/create-schema-ui.html?lang=en)を参照してください。
* **クラス**. [UI でクラスを作成および編集します](https://experienceleague.adobe.com/docs/experience-platform/xdm/ui/resources/classes.html?lang=en)。
* **フィールドグループ**&#x200B;を参照してください。[フィールドグル](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/composition.html?lang=en#field-group) ープの定義 [と、フィールドグループの詳細](https://experienceleague.adobe.com/docs/experience-platform/xdm/tutorials/create-schema-ui.html?lang=en#field-group)を参照してください。
* **IdentityMap**&#x200B;を参照してください。ID マップは、Adobe Experience Platform内のすべてのエンドユーザー ID のマップを表します。 [XDM フィールドディクショナリ ](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/field-dictionary.html?lang=en) の `xdm:identityMap` を参照してください。
* **SegmentMembership**&#x200B;を参照してください。[segmentMembership](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/field-dictionary.html?lang=en) XDM 属性は、プロファイルがどのセグメントに属しているかを知らせます。 `status` フィールドの 3 つの異なる値については、[Segment Membership Details schema field group](https://experienceleague.adobe.com/docs/experience-platform/xdm/field-groups/profile/segmentation.html) に関するドキュメントをお読みください。

## 概要 {#overview}

このページのコンテンツを、パートナーの宛先 ](./configuration-options.md) の残りの [ 設定オプションと共に使用します。 このページでは、Adobe Experience Platformから宛先に書き出されるデータのメッセージ形式について説明し、他のページでは、宛先への接続と認証に関する詳細について説明します。

Adobe Experience Platformは、様々なデータ形式で、大量の宛先にデータを書き出します。 宛先のタイプの例としては、広告プラットフォーム (Google)、ソーシャルネットワーク (Facebook)、クラウドストレージの場所 (Amazon S3、Azure Event Hubs) があります。

Experience Platformは、書き出されたメッセージの形式を、想定される形式に合わせて調整できます。 このカスタマイズを理解するには、次の概念が重要です。
* Adobe Experience Platformのソース (1) とターゲット (2) の XDM スキーマ
* パートナー側のメッセージの形式 (3)、
* この 2 つの間の変換レイヤー。[ メッセージ変換テンプレート ](./message-format.md#using-templating) を作成して定義できます。

![スキーマから JSON への変換](./assets/transformations-3-steps.png)

Experience Platform では、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。

宛先に対してデータをアクティブ化する場合は、データセットに使用するフィールドを、宛先の想定される形式に変換するスキーマにExperience Platformしてマッピングする必要があります。 Adobeは、会社がターゲットスキーマに追加できるカスタムフィールドグループを作成します。 フィールドグループのフィールドは、受け取ることのできる profile 属性フィールドによって異なります。

**ソース XDM スキーマ (1)**:顧客がスキーマで使用するExperience Platform。Experience Platformでは、宛先のアクティブ化ワークフローの [ マッピング手順 ](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-segment-streaming-destinations.html?lang=en#mapping) で、顧客はソーススキーマのフィールドを宛先のターゲットスキーマにマッピングします (2)。

**ターゲット XDM スキーマ (2)**:Adobeと共有する JSON 標準スキーマ (3) に基づいて、Adobeチームが宛先のカスタムスキーマを作成します。プロジェクト ](./overview.md#phased-approach) の [ 今後の段階では、独自で宛先のカスタムスキーマを作成できます。

**宛先プロファイル属性の JSON 標準スキーマ (3)**:お使いのプラットフォームがサ [ポート](https://json-schema.org/learn/miscellaneous-examples.html) するすべてのプロファイル属性とそのタイプ ( 例：オブジェクト、文字列、配列 )。宛先でサポートされるフィールドの例は、`firstName`、`lastName`、`gender`、`email`、`phone`、`productId`、`productName` などです。

上記のスキーマ変換に基づいて、ソース XDM スキーマとパートナー側のサンプルスキーマとの間でメッセージの構造がどのように変化するかを次に示します。

![変換メッセージの例](./assets/transformations-with-examples.png)

<br> 


## はじめに — 3 つの基本的な属性の変換 {#getting-started}

次の例では、Adobe Experience Platformの 3 つの共通のプロファイル属性を使用して、変換プロセスを示します。**名**、**姓**、**電子メールアドレス**。

>[!NOTE]
>
>顧客は、[ 宛先ワークフローのアクティブ化 ](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate-destinations.html#mapping) の **マッピング** 手順で、ソース XDM スキーマの属性をAdobe Experience Platform UI のパートナー XDM スキーマにマッピングします。

プラットフォームが次のようなメッセージ形式を受信できるとします。

```curl
POST https://YOUR_REST_API_URL/users/
Content-Type: application/json
Authorization: Bearer YOUR_REST_API_KEY

{
  "attributes":
    {
      "first_name": "Yours",
      "last_name": "Truly",
      "external_id": "yourstruly@adobe.com"
    }
}
```

メッセージ形式を考えると、対応する変換は次のようになります。

| Adobe側のパートナー XDM スキーマの属性 | 変換 | 側の HTTP メッセージの属性 |
|---------|----------|---------|
| `_your_custom_schema.firstName` | ` attributes.first_name` | `first_name` |
| `_your_custom_schema.lastName` | `attributes.last_name` | `last_name` |
| `personalEmail.address` | `attributes.external_id` | `external_id` |

## ID、属性、セグメントメンバーシップ変換に対するテンプレート言語の使用 {#using-templating}

Adobeは、[Jinjer](https://jinja.palletsprojects.com/en/2.11.x/) と同様のテンプレート言語を使用して、XDM スキーマのフィールドを、宛先でサポートされる形式に変換します。

この節では、入力 XDM スキーマからテンプレートを介して、宛先で受け入れられるペイロード形式にこれらの変換を出力する方法の例をいくつか示します。 以下の例は、次のように複雑さを増して分類されています。

1. 単純な変換の例。 [ プロファイル属性 ](./message-format.md#attributes)、[ セグメントメンバーシップ ](./message-format.md#segment-membership)、[ID](./message-format.md#identities) フィールドの単純な変換でテンプレートがどのように機能するかを説明します。
2. 上記のフィールドを組み合わせたテンプレートの複雑な例を増やしました。[ セグメントと ID を送信するテンプレートを作成 ](./message-format.md#segments-and-identities) および [ セグメント、ID、プロファイル属性を送信するテンプレートを作成 ](./message-format.md#segments-identities-attributes) します。
3. テンプレートには、集計キーが含まれます。 宛先の設定で [ 設定可能な集計 ](./destination-configuration.md#configurable-aggregation) を使用すると、Experience Platformは、セグメント ID、セグメントステータス、ID 名前空間などの条件に基づいて、宛先に書き出されたプロファイルをグループ化します。

### プロファイル属性 {#attributes}

書き出したプロファイル属性を宛先に変換するには、以下の JSON とコードサンプルを参照してください。

>[!IMPORTANT]
>
>Adobe Experience Platformで使用可能なすべてのプロファイル属性のリストについては、[XDM フィールドディクショナリ ](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/field-dictionary.html?lang=en) を参照してください。


**入力**

プロファイル 1:

```json
{
    "attributes": {
        "firstName": {
            "value": "Hermione"
    },
    "birthDate": {}
  }
}
```

プロファイル 2:

```json
{
  "attributes": {
    "firstName": {
      "value": "Harry"
    },
    "birthDate": {
        "value": "1980/07/31"
    }
  }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、[ 宛先サーバー設定 ](./server-and-template-configuration.md#template-specs) にテンプレートを挿入する前に、二重引用符 `""` などの無効な文字をエスケープする必要があります。 二重引用符のエスケープについて詳しくは、[JSON 標準 ](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf) の第 9 章を参照してください。

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            {% for attribute in profile.attributes %}
            "{{ attribute.key }}":
                {% if attribute.value is empty %}
                    null
                {% else %}
                    "{{ attribute.value.value }}"
                {% endif %}
            {% if not loop.last %},{% endif %}
            {% endfor %}
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**


```json
{
    "profiles": [
        {
            "firstName": "Hermione",
            "birthDate": null
        },
        {
            "firstName": "Harry",
            "birthDate": "1980/07/31"
        }
    ]
}
```

### セグメントのメンバーシップ {#segment-membership}

[segmentMembership](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/field-dictionary.html?lang=en) XDM 属性は、プロファイルがどのセグメントに属しているかを知らせます。
`status` フィールドの 3 つの異なる値については、[Segment Membership Details schema field group](https://experienceleague.adobe.com/docs/experience-platform/xdm/field-groups/profile/segmentation.html) に関するドキュメントをお読みください。

**入力**

プロファイル 1:

```json
{
  "segmentMembership": {
    "ups": {
      "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "realized"
      },
      "788d8874-8007-4253-92b7-ee6b6c20c6f3": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "existing"
      },
      "8f812592-3f06-416b-bd50-e7831848a31a": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "exited"
      }
    }
  }
}
```

プロファイル 2:

```json
{
  "segmentMembership": {
    "ups": {
      "32396e4b-16f6-4033-9702-fc69b5e24e7c": {
        "lastQualificationTime": "2021-08-20T17:23:04Z",
        "status": "realized"
      },
      "af854278-894a-4192-a96b-320fbf2623fd": {
        "lastQualificationTime": "2021-08-20T16:44:37Z",
        "status": "existing"
      },
      "66505bf9-bc08-4bac-afbc-8b6706650ea4": {
        "lastQualificationTime": "2019-08-20T17:23:04Z",
        "status": "realized"
      }
    }
  }
}
```

**テンプレート**


>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、[ 宛先サーバー設定 ](./server-and-template-configuration.md#template-specs) にテンプレートを挿入する前に、二重引用符 `""` などの無効な文字をエスケープする必要があります。 二重引用符のエスケープについて詳しくは、[JSON 標準 ](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf) の第 9 章を参照してください。

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            "AdobeExperiencePlatformSegments": {
                "add": [
                {% for segment in profile.segmentMembership.ups | added %}
                "{{ segment.key }}"{% if not loop.last %},{% endif %}
                {% endfor %}
                ],
                "remove": [
                {# Alternative syntax for filtering segments by status: #}
                {% for segment in removedSegments(profile.segmentMembership.ups) %}
                "{{ segment.key }}"{% if not loop.last %},{% endif %}
                {% endfor %}
                ]
            }
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**

```json
{
    "profiles": [
        {
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075",
                    "788d8874-8007-4253-92b7-ee6b6c20c6f3"
                ],
                "remove": [
                    "8f812592-3f06-416b-bd50-e7831848a31a"
                ]
            }
        },
        {
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "32396e4b-16f6-4033-9702-fc69b5e24e7c",
                    "af854278-894a-4192-a96b-320fbf2623fd",
                    "66505bf9-bc08-4bac-afbc-8b6706650ea4"
                ],
                "remove": [
                ]
            }
        }
    ]
}
```

### ID {#identities}

Experience Platformの ID について詳しくは、[ID 名前空間の概要 ](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja) を参照してください。

**入力**

プロファイル 1:

```json
{
    "identityMap": {
        "email": [
            {
                "id": "johndoe@example.com"
            },
            {
                "id": "jd@example.com"
            }
        ],
        "external_id": [
            {
                "id": "123456"
            }
        ]
    }
}
```

プロファイル 2:

```json
{
    "identityMap": {
        "email": [
            {
                "id": "jane.doe@example.com"
            }
        ]
    }
}
```

**テンプレート**


>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、[ 宛先サーバー設定 ](./server-and-template-configuration.md#template-specs) にテンプレートを挿入する前に、二重引用符 `""` などの無効な文字をエスケープする必要があります。 二重引用符のエスケープについて詳しくは、[JSON 標準 ](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf) の第 9 章を参照してください。

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            "identities": [
                {% for email in profile.identityMap.email %}
                {
                    "type": "email",
                    "id": "{{ email.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}

                {# Add a comma only if you have both emails and external_ids. #}
                {% if profile.identityMap.email is not empty and profile.identityMap.external_id is not empty %}
                    ,
                {% endif %}

                {% for external in profile.identityMap.external_id %}
                {
                    "type": "external_id",
                    "id": "{{ external.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}
            ]
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**

```json
{
    "profiles": [
        {
            "identities": [
                {
                    "type": "email",
                    "id": "johndoe@example.com"
                },
                {
                    "type": "email",
                    "id": "jd@example.com"
                },
                {
                    "type": "external_id",
                    "id": "123456"
                }
            ]
        },
        {
            "identities": [
                {
                    "type": "email",
                    "id": "jane.doe@example.com"
                }
            ]
        }
    ]
}
```


### セグメントと ID を送信するテンプレートの作成 {#segments-and-identities}

この節では、AdobeXDM スキーマとパートナー宛先スキーマの間でよく使用される変換の例を示します。
次の例は、セグメントのメンバーシップと ID の形式を変換して、宛先に出力する方法を示しています。

**入力**

プロファイル 1:

```json
{
    "identityMap": {
        "email": [
            {
                "id": "johndoe@example.com"
            },
            {
                "id": "jd@example.com"
            }
        ],
        "external_id": [
            {
                "id": "123456"
            }
        ]
    },
    "segmentMembership": {
        "ups": {
            "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "realized"
            },
            "788d8874-8007-4253-92b7-ee6b6c20c6f3": {
              "lastQualificationTime": "2019-11-20T13:15:49Z",
              "status": "existing"
            },
            "8f812592-3f06-416b-bd50-e7831848a31a": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "exited"
            }
        }
    }
}
```

プロファイル 2:

```json
{
    "identityMap": {
        "email": [
            {
                "id": "jane.doe@example.com"
            }
        ]
    },
    "segmentMembership": {
        "ups": {
            "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
                "lastQualificationTime": "2021-08-31T10:01:42Z",
                "status": "realized"
            }
        }
    }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、[ 宛先サーバー設定 ](./server-and-template-configuration.md#template-specs) にテンプレートを挿入する前に、二重引用符 `""` などの無効な文字をエスケープする必要があります。 二重引用符のエスケープについて詳しくは、[JSON 標準 ](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf) の第 9 章を参照してください。

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            "identities": [
                {% for email in profile.identityMap.email %}
                {
                    "type": "email",
                    "id": "{{ email.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}
                
                {# Add a comma only if you have both emails and external_ids. #}
                {% if profile.identityMap.email is not empty and profile.identityMap.external_id is not empty %}
                    ,
                {% endif %}
                
                {% for external in profile.identityMap.external_id %}
                {
                    "type": "external_id",
                    "id": "{{ external.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    {% for segment in profile.segmentMembership.ups | added %}
                    "{{ segment.key }}"{% if not loop.last %},{% endif %}
                    {% endfor %}
                ],
                "remove": [
                    {# Alternative syntax for filtering segments by status: #}
                    {% for segment in removedSegments(profile.segmentMembership.ups) %}
                    "{{ segment.key }}"{% if not loop.last %},{% endif %}
                    {% endfor %}
                ]
            }
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**

以下の `json` は、Adobe Experience Platformから書き出されたデータを表しています。

```json
{
    "profiles": [
        {
            "identities": [
                {
                    "type": "email",
                    "id": "johndoe@example.com"
                },
                {
                    "type": "email",
                    "id": "jd@example.com"
                },
                {
                    "type": "external_id",
                    "id": "123456"
                }
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075",
                    "788d8874-8007-4253-92b7-ee6b6c20c6f3"
                ],
                "remove": [
                    "8f812592-3f06-416b-bd50-e7831848a31a"
                ]
            }
        },
        {
            "identities": [
                {
                    "type": "email",
                    "id": "jane.doe@example.com"
                }
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075"
                ],
                "remove": []
            }
        }
    ]
}
```

### セグメント、ID およびプロファイル属性を送信するテンプレートの作成 {#segments-identities-attributes}

この節では、AdobeXDM スキーマとパートナー宛先スキーマの間でよく使用される変換の例を示します。

もう 1 つの一般的な使用例は、セグメントのメンバーシップ、ID( 例：電子メールアドレス、電話番号、広告 ID)、プロファイル属性。 この方法でデータをエクスポートするには、以下の例を参照してください。

**入力**

プロファイル 1:

```json
{
    "attributes": {
        "firstName": {
            "value": "Hermione"
        },
        "birthDate": {}
    },
    "identityMap": {
        "email": [
            {
                "id": "johndoe@example.com"
            },
            {
                "id": "jd@example.com"
            }
        ],
        "external_id": [
            {
                "id": "123456"
            }
        ]
    },
    "segmentMembership": {
        "ups": {
            "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "realized"
            },
            "788d8874-8007-4253-92b7-ee6b6c20c6f3": {
              "lastQualificationTime": "2019-11-20T13:15:49Z",
              "status": "existing"
            },
            "8f812592-3f06-416b-bd50-e7831848a31a": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "exited"
            }
        }
    }
}
```

プロファイル 2:

```json
{
    "attributes": {
        "firstName": {
            "value": "Harry"
        },
        "birthDate": {
            "value": "1980/07/31"
        }
    },
    "identityMap": {
        "email": [
            {
                "id": "harry.p@example.com"
            }
        ]
    },
    "segmentMembership": {
        "ups": {
            "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "realized"
            }
        }
    }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、[ 宛先サーバー設定 ](./server-and-template-configuration.md#template-specs) にテンプレートを挿入する前に、二重引用符 `""` などの無効な文字をエスケープする必要があります。 二重引用符のエスケープについて詳しくは、[JSON 標準 ](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf) の第 9 章を参照してください。

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            "attributes": {
            {% for attribute in profile.attributes %}
                "{{ attribute.key }}":
                    {% if attribute.value is empty %}
                        null
                    {% else %}
                        "{{ attribute.value.value }}"
                    {% endif %}
                {% if not loop.last %},{% endif %}
            {% endfor %}
            },
            "identities": [
                {% for email in profile.identityMap.email %}
                {
                    "type": "email",
                    "id": "{{ email.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}

                {# Add a comma only if we have both emails and external_ids. #}
                {% if profile.identityMap.email is not empty and profile.identityMap.external_id is not empty %}
                    ,
                {% endif %}

                {% for external in profile.identityMap.external_id %}
                {
                    "type": "external_id",
                    "id": "{{ external.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                {% for segment in profile.segmentMembership.ups | added %}
                    "{{ segment.key }}"{% if not loop.last %},{% endif %}
                {% endfor %}
                ],
                "remove": [
                {# Alternative syntax for filtering segments by status: #}
                {% for segment in removedSegments(profile.segmentMembership.ups) %}
                    "{{ segment.key }}"{% if not loop.last %},{% endif %}
                {% endfor %}
                ]
            }
        }
    ]
}
```

**結果**

以下の `json` は、Adobe Experience Platformから書き出されたデータを表しています。

```json
{
    "profiles": [
        {
            "attributes": {
                "firstName": "Hermione",
                "birthDate": null
            },
            "identities": [
                {
                    "type": "email",
                    "id": "johndoe@example.com"
                },
                {
                    "type": "email",
                    "id": "jd@example.com"
                },
                {
                    "type": "external_id",
                    "id": "123456"
                }
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075",
                    "788d8874-8007-4253-92b7-ee6b6c20c6f3"
                ],
                "remove": [
                    "8f812592-3f06-416b-bd50-e7831848a31a"
                ]
            }
        },
        {
            "attributes": {
                "firstName": "Harry",
                "birthDate": "1980/07/21"
            },
            "identities": [
                {
                    "type": "email",
                    "id": "harry.p@example.com"
                }
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075"
                ],
                "remove": []
            }
        }
    ]
}
```

### テンプレートに集計キーを含めて、様々な条件でグループ化してエクスポートされたプロファイルにアクセスします。 {#template-aggregation-key}

宛先の設定で [ 設定可能な集計 ](./destination-configuration.md#configurable-aggregation) を使用すると、セグメント ID、セグメントエイリアス、セグメントメンバーシップ、ID 名前空間などの条件に基づいて、宛先に書き出されたプロファイルをグループ化できます。

メッセージ変換テンプレートでは、次の節の例に示すように、上記の集計キーにアクセスできます。 これにより、宛先で期待される形式に合わせて、Experience Platformから書き出される HTTP メッセージを書式設定できます。

#### テンプレートでのセグメント ID 集計キーの使用 {#aggregation-key-segment-id}

[ 設定可能な集計 ](./destination-configuration.md#configurable-aggregation) を使用し、`includeSegmentId` を true に設定した場合、宛先に書き出される HTTP メッセージ内のプロファイルはセグメント ID でグループ化されます。 以下で、テンプレートのセグメント ID にアクセスする方法を参照してください。

**入力**

次の 4 つのプロファイルについて考えてみましょう。
* 最初の 2 つは、セグメント ID `788d8874-8007-4253-92b7-ee6b6c20c6f3` を持つセグメントの一部です。
* 3 つ目のプロファイルは、セグメント ID `8f812592-3f06-416b-bd50-e7831848a31a` を持つセグメントの一部です。
* 4 つ目のプロファイルは、上記の両方のセグメントの一部です。

プロファイル 1:

```json
{
   "attributes":{
      "firstName":{
         "value":"Hermione"
      }
   },
   "segmentMembership":{
      "ups":{
         "788d8874-8007-4253-92b7-ee6b6c20c6f3":{
            "lastQualificationTime":"2020-11-20T13:15:49Z",
            "status":"existing"
         }
      }
   }
}
```

プロファイル 2:

```json
{
   "attributes":{
      "firstName":{
         "value":"Harry"
      }
   },
   "segmentMembership":{
      "ups":{
         "788d8874-8007-4253-92b7-ee6b6c20c6f3":{
            "lastQualificationTime":"2020-11-20T13:15:49Z",
            "status":"existing"
         }
      }
   }
}
```

プロファイル 3:

```json
{
   "attributes":{
      "firstName":{
         "value":"Tom"
      }
   },
   "segmentMembership":{
      "ups":{
         "8f812592-3f06-416b-bd50-e7831848a31a":{
            "lastQualificationTime":"2021-02-20T12:00:00Z",
            "status":"existing"
         }
      }
   }
}
```

プロファイル 4:

```json
{
   "attributes":{
      "firstName":{
         "value":"Jerry"
      }
   },
   "segmentMembership":{
      "ups":{
         "8f812592-3f06-416b-bd50-e7831848a31a":{
            "lastQualificationTime":"2021-02-20T12:00:00Z",
            "status":"existing"
         },
         "788d8874-8007-4253-92b7-ee6b6c20c6f3":{
            "lastQualificationTime":"2020-11-20T13:15:49Z",
            "status":"existing"
         }
      }
   }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、[ 宛先サーバー設定 ](./server-and-template-configuration.md#template-specs) にテンプレートを挿入する前に、二重引用符 `""` などの無効な文字をエスケープする必要があります。 二重引用符のエスケープについて詳しくは、[JSON 標準 ](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf) の第 9 章を参照してください。

次に、テンプレートで `audienceId` がセグメント ID にアクセスする方法を示します。 これは、宛先の分類でセグメントメンバーシップに `audienceId` を使用していることを前提としています。 独自の分類に応じて、他の任意のフィールド名を代わりに使用できます。

```python
{
    "audienceId": "{{ input.aggregationKey.segmentId }}",
    "profiles": [
        {% for profile in input.profiles %}
        {
            "first_name": "{{ profile.attributes.firstName.value }}"
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**

宛先に書き出すと、プロファイルは、セグメント ID に基づいて 2 つのグループに分割されます。

```json
{
   "audienceId":"788d8874-8007-4253-92b7-ee6b6c20c6f3",
   "profiles":[
      {
         "firstName":"Hermione"
      },
      {
         "firstName":"Harry"
      },
      {
         "firstName":"Jerry"
      }
   ]
}
```

```json
{
   "audienceId":"8f812592-3f06-416b-bd50-e7831848a31a",
   "profiles":[
      {
         "firstName":"Tom"
      },
      {
         "firstName":"Jerry"
      }
   ]
}
```

#### テンプレートでのセグメントエイリアス集計キーの使用 {#aggregation-key-segment-alias}

[ 設定可能な集計 ](./destination-configuration.md#configurable-aggregation) を使用し、`includeSegmentId` を true に設定した場合は、テンプレートのセグメントエイリアスにアクセスすることもできます。

テンプレートの下に行を追加して、セグメントエイリアスでグループ化されてエクスポートされたプロファイルにアクセスします。

```python
customerList={{input.aggregationKey.segmentAlias}}
```

#### テンプレートでのセグメントステータス集計キーの使用 {#aggregation-key-segment-status}

[ 設定可能な集計 ](./destination-configuration.md#configurable-aggregation) を使用し、`includeSegmentId` と `includeSegmentStatus` を true に設定した場合、テンプレートのセグメントステータスにアクセスして、プロファイルの追加または削除の有無に基づいて、宛先にエクスポートされる HTTP メッセージ内のプロファイルをグループ化できます。

次のような値を選択できます。

* 実現
* 既存
* 出口

上記の値に基づいて、テンプレートに下の行を追加し、セグメントに対してプロファイルを追加または削除します。

```python
action={% if input.aggregationKey.segmentStatus == "exited" %}REMOVE{% else %}ADD{% endif%}
```

#### テンプレートで ID 名前空間集計キーを使用する {#aggregation-key-identity}

次の例では、宛先設定の [ 設定可能な集計 ](./destination-configuration.md#configurable-aggregation) が、書き出されたプロファイルを ID 名前空間別に `"namespaces": ["email", "phone"]` および `"namespaces": ["GAID", "IDFA"]` の形式で集計するように設定されています。 この処理方法については、[ 宛先設定 API リファレンス ](./destination-configuration-api.md) の `groups` パラメーターを参照してください。

**入力**

プロファイル 1:

```json
{
   "identityMap":{
      "email":[
         {
            "id":"e1@example.com"
         },
         {
            "id":"e2@example.com"
         }
      ],
      "phone":[
         {
            "id":"+40744111222"
         }
      ],
      "IDFA":[
         {
            "id":"AEBE52E7-03EE-455A-B3C4-E57283966239"
         }
      ],
      "GAID":[
         {
            "id":"e4fe9bde-caa0-47b6-908d-ffba3fa184f2"
         }
      ]
   }
}
```

プロファイル 2:

```json
{
   "identityMap":{
      "email":[
         {
            "id":"e3@example.com"
         }
      ],
      "phone":[
         {
            "id":"+40744333444"
         },
         {
            "id":"+40744555666"
         }
      ],
      "IDFA":[
         {
            "id":"134GHU45-34HH-GHJ7-K0H8-LHN665998NN0"
         }
      ],
      "GAID":[
         {
            "id":"47bh00i9-8jv6-334n-lll8-nb7f24sghg76"
         }
      ]
   }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、[ 宛先サーバー設定 ](./server-and-template-configuration.md#template-specs) にテンプレートを挿入する前に、二重引用符 `""` などの無効な文字をエスケープする必要があります。 二重引用符のエスケープについて詳しくは、[JSON 標準 ](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf) の第 9 章を参照してください。

`input.aggregationKey.identityNamespaces` は以下のテンプレートで使用されています。

```python
{
            "profiles": [
            {% for profile in input.profiles %}
            {
                {% for ns in input.aggregationKey.identityNamespaces %}
                "{{ns}}": [
                    {% for id in profile.identityMap[ns] %}
                    "{{id.id}}"{% if not loop.last %},{% endif %}
                    {% endfor %}
                ]{% if not loop.last %},{% endif %}
                {% endfor %}
            }{% if not loop.last %},{% endif %}
            {% endfor %}
        ]
}
```

**結果**

宛先に書き出すと、プロファイルは、ID 名前空間（1 つのグループの電子メールと電話、別のグループの GAID と IDFA）に基づいて 2 つのグループに分割されます。

```json
{
   "profiles":[
      {
         "email":[
            "e1@example.com",
            "e2@example.com"
         ],
         "phone":[
            "+40744111222"
         ]
      },
      {
         "email":[
            "e3@example.com"
         ],
         "phone":[
            "+40744333444",
            "+40744555666"
         ]
      }
   ]
}
```

```json
{
   "profiles":[
      {
         "IDFA":[
            "AEBE52E7-03EE-455A-B3C4-E57283966239"
         ],
         "GAID":[
            "e4fe9bde-caa0-47b6-908d-ffba3fa184f2"
         ]
      },
      {
         "IDFA":[
            "134GHU45-34HH-GHJ7-K0H8-LHN665998NN0"
         ],
         "GAID":[
            "47bh00i9-8jv6-334n-lll8-nb7f24sghg76"
         ]
      }
   ]
}
```

#### URL テンプレートでの集計キーの使用 {#aggregation-key-url-template}

使用例に応じて、次に示すように、URL にここで説明する集計キーも使用できます。

```python
https://api.example.com/audience/{{input.aggregationKey.segmentId}}
```

### リファレンス：変換テンプレートで使用されるコンテキストと関数 {#reference}

テンプレートに指定したコンテキストには、`input`（この呼び出しで書き出されるプロファイル/データ）と `destination`(Adobeがデータを送信する宛先に関するデータ。すべてのプロファイルで有効 ) が含まれます。

次の表に、上記の例の関数の説明を示します。

| 関数 | 説明 |
|---------|----------|
| `input.profile` | プロファイル。[JsonNode](http://fasterxml.github.io/jackson-databind/javadoc/2.11/com/fasterxml/jackson/databind/node/JsonNodeType.html) で表されます。 このページで後述するパートナー XDM スキーマに従います。 |
| `destination.segmentAliases` | Adobe Experience Platform名前空間のセグメント ID からパートナーのシステムのセグメントエイリアスにマッピングします。 |
| `destination.segmentNames` | Adobe Experience Platform名前空間のセグメント名からパートナーのシステムのセグメント名にマッピングします。 |
| `addedSegments(listOfSegments)` | ステータス `realized` または `existing` を持つセグメントのみを返します。 |
| `removedSegments(listOfSegments)` | ステータス `exited` を持つセグメントのみを返します。 |

<!--

## What Adobe needs from you to set up your destination {#what-adobe-needs}

Based on the transformations outlined in the sections above, Adobe needs the following information to set up your destination:

* Considering *all* the fields that your platform can receive, Adobe needs the standard JSON schema that corresponds to your expected message format. Having the template allows Adobe to define transformations and to create a custom XDM schema for your company, which customers would use to export data to your destination.

-->
