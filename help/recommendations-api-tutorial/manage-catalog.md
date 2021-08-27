---
title: APIを使用したRecommendationsカタログの管理方法
description: チュートリアルのこの部分では、Adobe Target APIを使用してRecommendationsカタログ内のエンティティを作成、更新、保存、取得、削除するために必要な手順を、開発者に説明します。
role: Developer
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Recommendations, Administration & Configuration
doc-type: tutorial
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: 8060b69b-e8e5-4fe7-895f-742410d8ed45
source-git-commit: d1517f0763290eb61a9e4eef4f2eb215a9cdd667
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 2%

---

# APIを使用した[!DNL Recommendations]カタログの管理

この時点で、JWT認証フローを使用してアクセストークンを生成し、Adobe Target Admin APIをAdobe I/Oで使用する方法を学びました。

[Recommendations API](https://developers.adobetarget.com/api/recommendations/)を使用して、recommendationsカタログの品目の追加、更新、削除をおこなうことができます。 残りのAdobe Target Admin APIと同様に、[!DNL Recommendations] APIでは認証が必要です。

>[!TIP]
>
>**[!UICONTROL IMSを送信します。JWT認証用のアクセストークンを更新する必要が生じた場合は常に、ユーザートークン]**&#x200B;リクエストを介して+認証を生成します。これは、認証が24時間後に期限切れになるためです。 手順については、「[AdobeAPI認証の設定](../apis/configure-io-target-integration.md)」を参照してください。

![JWT3ff](assets/configure-io-target-jwt3ff.png)

>[!NOTE]
>
>先に進む前に、[Recommendations Postmanコレクション](https://developers.adobetarget.com/api/recommendations/#section/Postman)を取得します。

## エンティティの保存APIを使用した項目の作成と更新

製品ページで実行されるCSV製品フィードや[!DNL Target]リクエストの代わりにAPIを使用して[!DNL Recommendations]製品データベースを設定するには、[Save Entities API](https://developers.adobetarget.com/api/recommendations/#operation/saveEntities)を使用します。 このリクエストは、1つの[!DNL Target]環境内の項目を追加または更新します。 構文は次のとおりです。

```
POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities
```

例えば、「エンティティの保存」を使用すると、在庫や価格のしきい値など、特定のしきい値を満たすたびに品目を更新し、品目にフラグを付けてレコメンデーションされないようにできます。

1. **[!DNL Target]/ [!UICONTROL セットアップ] / [!UICONTROL ホスト] / [!UICONTROL 環境]**&#x200B;に移動して、項目を追加または更新する[!DNL Target]環境IDを取得します。

   ![SaveEntities1](assets/SaveEntities01.png)

2. `TENANT_ID`と`API_KEY`が、前に確立したPostman環境変数を参照していることを確認します。 以下の画像を比較に使用してください。 必要に応じて、APIリクエストのヘッダーとパスを、以下の画像のヘッダーと一致するように変更します。

   ![SaveEntities3](assets/SaveEntities03.png)

3. JSONに&#x200B;**raw**&#x200B;コードを&#x200B;**本文**&#x200B;に入力します。 `environment`変数を使用して、必ず環境IDを指定してください。 （以下の例では、環境IDは6781です）。

   ![SaveEntities4.png](assets/SaveEntities04.png)

   >!![NOTE]
   以下に、Toaster Oven製品の関連エンティティ値を含むentity.id kit2001を環境6781に追加するサンプルJSONを示します。

   ```
      {
      "entities": [{
              "name": "Toaster Oven",
              "id": "kit2001",
              "environment": 6781,
              "categories": [
                  "housewares:appliances"
              ],
              "attributes": {
                  "inventory": 77,
                  "margin": 23,
                  "message": "crashing helicopter",
                  "pageUrl": "www.foobar.foo.com/helicopter.html",
                  "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                  "value": 19.2
              }
          }]
      }
   ```

4. 「**送信**」をクリックします。次の応答が返されます。

   ![SaveEntities5.png](assets/SaveEntities05.png)

JSONオブジェクトは、複数の製品を送信するように拡大/縮小できます。 例えば、このJSONは2つのエンティティを指定します。

```
    {
        "entities": [{
                "name": "Toaster Oven",
                "id": "kit2001",
                "environment": 6781,
                "categories": [
                    "housewares:appliances"
                ],
                "attributes": {
                    "inventory": 89,
                    "margin": 11,
                    "message": "Toaster Oven",
                    "pageUrl": "www.foobar.foo.com/helicopter.html",
                    "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                    "value": 102.5
                }
            },
            {
                "name": "Blender",
                "id": "kit2002",
                "environment": 6781,
                "categories": [
                    "housewares:appliances"
                ],
                "attributes": {
                    "inventory": 36,
                    "margin": 5,
                    "message": "Blender",
                    "pageUrl": "www.foobar.foo.com/helicopter.html",
                    "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                    "value": 54.5
                }
            }
        ]
    }
```

1. 今度は君の番だ！ **エンティティの保存** APIを使用して、次の項目をカタログに追加します。 上記のサンプルJSONを出発点として使用します。 （JSONを拡張して、追加のエンティティを含める必要があります）。

   ![SaveEntities5.png](assets/SaveEntities06.png)

おお、最後の2つのアイテムは属していないようだ。 ここでは、**Get Entity** APIを使用して検証し、必要に応じて&#x200B;**Delete Entities** APIを使用して削除します。

## エンティティ取得APIを使用した品目の詳細の取得

既存の項目の詳細を取得するには、[Get Entity API](https://developers.adobetarget.com/api/recommendations/#operation/getEntity)を使用します。 構文は次のとおりです。

```
GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities/[entity.id]
```

エンティティの詳細は、一度に1つのエンティティに対してのみ取得できます。 「エンティティを取得」を使用して、カタログでの更新が期待どおりにおこなわれたことを確認したり、カタログの内容を監査したりできます。

1. APIリクエストで、変数`entityId`を使用してエンティティIDを指定します。 次の例は、entityId=kit2004のエンティティの詳細を返します。

   ![GetEntity1](assets/GetEntity1.png)

2. `TENANT_ID`と`API_KEY`が、前に確立したPostman環境変数を参照していることを確認します。 以下の画像を比較に使用してください。 必要に応じて、APIリクエストのヘッダーとパスを、以下の画像のヘッダーと一致するように変更します。

   ![GetEntity2](assets/GetEntity2.png)

3. リクエストを送信します。

   ![GetEntity3](assets/GetEntity3.png)
上の例に示すように、エンティティが見つからなかったことを示すエラーが表示された場合は、リクエストを正しい環境に送信していることを確認し [!DNL Target] ます。

   >[!NOTE]
   環境が明示的に指定されていない場合、Get Entityは、[デフォルト環境](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html?lang=en)からのみエンティティを取得しようとします。 デフォルト環境以外の環境から取り込む場合は、環境IDを指定する必要があります。

4. 必要に応じて、`environmentId`パラメーターを追加し、要求を再送信します。

   ![GetEntity4](assets/GetEntity4.png)

5. 別の&#x200B;**Get Entity**&#x200B;リクエストを送信します。今回は、entityId=kit2005のエンティティを調べます。

   ![GetEntity5](assets/GetEntity5.png)

これらのエンティティをカタログから削除する必要があると判断したとします。 **エンティティを削除** APIを使用します。

## エンティティ削除APIを使用した品目の削除

カタログから品目を削除するには、[エンティティを削除API](https://developers.adobetarget.com/api/recommendations/#operation/deleteEntities)を使用します。 構文は次のとおりです。

```
DELETE https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities?ids=[comma-delimited-entity-ids]&environment=[environmentId]
```

>[!WARNING]
このAPIは、指定したIDで参照されているエンティティを削除します。
エンティティIDを指定しない場合、指定した環境内のすべてのエンティティが削除されます。 環境IDが指定されていない場合、エンティティはすべての環境から削除されます。 注意して使用してください。

1. **[!DNL Target]/ [!UICONTROL セットアップ] / [!UICONTROL ホスト] / [!UICONTROL 環境]**&#x200B;に移動して、項目を削除する[!DNL Target]環境IDを取得します。

   ![DeleteEntities1](assets/SaveEntities01.png)

2. APIリクエストで、`&ids=[comma-delimited-entity-ids]`（クエリパラメーター）という構文を使用して、削除するエンティティのエンティティIDを指定します。 複数のエンティティを削除する場合は、コンマでIDを区切ります。

   ![DeleteEntities2](assets/DeleteEntities2.png)

3. 構文`&environment=[environmentId]`を使用して環境IDを指定します。そうしないと、すべての環境のエンティティが削除されます。

   ![DeleteEntities3](assets/DeleteEntities3.png)

4. `TENANT_ID`と`API_KEY`が、前に確立したPostman環境変数を参照していることを確認します。 以下の画像を比較に使用してください。 必要に応じて、APIリクエストのヘッダーとパスを、以下の画像のヘッダーと一致するように変更します。

   ![DeleteEntities4](assets/DeleteEntities4.png)

5. リクエストを送信します。

   ![DeleteEntities5](assets/DeleteEntities5.png)

6. 「**エンティティ**&#x200B;を取得」を使用して結果を確認します。これは、削除されたエンティティが見つからないことを示すはずです。

   ![DeleteEntities6](assets/DeleteEntities6.png)

   ![DeleteEntities6](assets/DeleteEntities7.png)

おめでとう！ [!DNL Recommendations] APIを使用して、カタログ内のエンティティの作成、更新、削除、詳細の取得をおこなえるようになりました。 次の節では、カスタム条件の管理方法について説明します。

[次：「カスタム条件の管理」>](manage-custom-criteria.md)
