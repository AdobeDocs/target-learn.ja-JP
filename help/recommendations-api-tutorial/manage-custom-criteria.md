---
title: カスタム条件の管理方法
description: チュートリアルのこの部分では、Adobe Target API を使用してAdobe Target Recommendationsの条件を管理、作成、リスト、編集、取得、削除するために必要な手順を開発者に説明します。
role: Developer
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Recommendations, Administration & Configuration
doc-type: tutorial
kt: 3815
author: Judy Kim
exl-id: ee63bd3e-200a-4c08-b364-9f17a479033b
source-git-commit: 342e02562b5296871638c1120114214df6115809
workflow-type: tm+mt
source-wordcount: '941'
ht-degree: 1%

---

# カスタム条件の管理

[!DNL Recommendations] が提供するアルゴリズムが、プロモーションしたい特定の項目を表示できない場合があります。 このような場合、カスタム条件を使用すると、特定のキー品目またはカテゴリに対してレコメンデーション品目の特定のセットを提供できます。 主要品目またはカテゴリと推奨品目の間のマッピングを定義し、そのマッピングをカスタム条件としてインポートします。 このプロセスについては、[ カスタム条件のドキュメント ](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/recommendations-csv.html?lang=en) で説明しています。 このドキュメントで述べたように、 [!DNL Target] ユーザーインターフェイス (UI) を使用して、カスタム条件の作成、編集、削除をおこなうことができます。 ただし、[!DNL Target] には、カスタム条件をより詳細に管理できるカスタム条件 API のセットも用意されています。

>[!IMPORTANT]
>
>カスタム条件については、次の使用ガイドラインに従います。
>
> API を使用して、特定のカスタム条件に対してすべての操作（作成、編集、削除）をおこなうか、UI を使用してすべての操作（作成、編集、削除）をおこないます。 UI と API を組み合わせてカスタム条件を管理すると、情報が競合したり、予期しない結果が生じる場合があります。 例えば、UI でカスタム条件を作成し、API で編集した場合、UI では更新されませんが、API で表示されるように、バックエンドで更新されます。

## カスタム条件の作成

[ カスタム条件を作成 API](https://developers.adobetarget.com/api/recommendations/#operation/createCriteriaCustom) を使用してカスタム条件を作成する場合の構文は次のとおりです。

`POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom`

>[!WARNING]
>
>カスタム条件を作成 API を使用して作成したカスタム条件は、この演習で説明するように、UI に表示され、そこで保持されます。 UI から編集や削除を行うことはできません。 API **を使用して** を編集または削除できますが、どちらの方法でも、引き続き [!DNL Target] UI に表示されます。 UI での編集または削除のオプションを維持するには、カスタム条件の作成 API を使用するのではなく、[ ドキュメント ](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/recommendations-csv.html?lang=en) ごとに UI を使用してカスタム条件を作成します。

上記の警告を読んだ後で、このチュートリアルを続行し、UI から削除できない新しいカスタム条件を作成することに慣れてください。

1. **カスタム条件** の `TENANT_ID` と `API_KEY` を確認し、前に確立した Postman 環境変数を参照します。 以下の画像を比較に使用してください。

   ![CustomCriteria1 の作成](assets/CreateCustomCriteria1.png)

2. **Body** を、カスタム条件の CSV ファイルの場所を定義する **raw** JSON として追加します。 [ カスタム条件の作成 API](https://developers.adobetarget.com/api/recommendations/#operation/getAllCriteriaCustom) のドキュメントに記載されている例をテンプレートとして使用し、必要に応じて `environmentId` とその他の値を指定します。 この例では、キーとして LAST_PURCHASED を使用します。

   ![CustomCriteria2 の作成](assets/CreateCustomCriteria2.png)

3. リクエストを送信し、作成したカスタム条件の詳細を含む応答を観察します。

   ![CustomCriteria3 の作成](assets/CreateCustomCriteria3.png)

4. カスタム条件が作成されたことを確認するには、Adobe Target内で **[!UICONTROL Recommendations] / [!UICONTROL  条件]** に移動し、名前で条件を検索するか、次の手順で **カスタム条件 API のリスト** を使用します。

   ![CustomCriteria4 の作成](assets/CreateCustomCriteria4.png)

この場合、エラーが発生します。 **カスタム条件のリスト API** を使用して、カスタム条件をより詳細に調べ、エラーを調べます。

## カスタム条件のリスト

すべてのカスタム条件のリストと各条件の詳細を取得するには、[ カスタム条件のリスト API](https://developers.adobetarget.com/api/recommendations/#operation/getAllCriteriaCustom) を使用します。 構文は次のとおりです。

`GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom`

1. `TENANT_ID` と `API_KEY` を前と同じように確認し、要求を送信します。 応答では、カスタム条件の ID と、前述のエラーメッセージに関する詳細をメモします。
   ![ListCustomCriteria](assets/ListCustomCriteria.png)

この場合、サーバー情報が正しくない、つまり [!DNL Target] がカスタム条件の定義を含む CSV ファイルにアクセスできないことが原因で、エラーが発生します。 カスタム条件を編集して、これを修正します。

## カスタム条件の編集

カスタム条件定義の詳細を変更するには、[ カスタム条件を編集 API](https://developers.adobetarget.com/api/recommendations/#operation/updateCriteriaCustom) を使用します。 構文は次のとおりです。

`POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. `TENANT_ID` と `API_KEY` を前と同様に確認します。
   ![EditCustomCriteria1](assets/EditCustomCriteria1.png)

1. 編集する（単一の）カスタム条件の条件 ID を指定します。
   ![EditCustomCriteria2](assets/EditCustomCriteria2.png)

1. 本文で、更新された JSON に正しいサーバー情報を指定します。 （この手順では、アクセス可能なサーバーへの FTP アクセスを指定します）。
   ![EditCustomCriteria3](assets/EditCustomCriteria3.png)

1. リクエストを送信し、応答をメモします。
   ![EditCustomCriteria4](assets/EditCustomCriteria4.png)

**カスタム条件 API** を使用して、更新されたカスタム条件の成功を検証します。

## カスタム条件の取得

特定のカスタム条件のカスタム条件の詳細を表示するには、[ カスタム条件 API の取得 ](https://developers.adobetarget.com/api/recommendations/#operation/getCriteriaCustom) を使用します。 構文は次のとおりです。

`GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. 詳細を取得するカスタム条件の条件 ID を指定します。 リクエストを送信し、応答を確認します。
   ![GetCustomCriteria.png](assets/GetCustomCriteria.png)
1. 成功を確認します。 （この場合は、それ以上 FTP エラーがないことを確認します）。
   ![GetCustomCriteria1.png](assets/GetCustomCriteria1.png)
1. （オプション）UI に更新が正しく反映されていることを確認します。
   ![GetCustomCriteria2.png](assets/GetCustomCriteria2.png)

## カスタム条件の削除

前述の条件 ID を使用して、[ カスタム条件を削除 API](https://developers.adobetarget.com/api/recommendations/#operation/deleteCriteriaCustom) を使用してカスタム条件を削除します。 構文は次のとおりです。

`DELETE https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. 削除する（単一の）カスタム条件の条件 ID を指定します。 「**送信**」をクリックします。
   ![DeleteCustomCriteria1](assets/DeleteCustomCriteria1.png)

1. 「カスタム条件を取得」を使用して、条件が削除されたことを確認します。
   ![DeleteCustomCriteria2](assets/DeleteCustomCriteria2.png)
この場合、404 エラーが発生すると、削除された条件が見つからないことが示されます。

>[!NOTE]
>なお、この条件はカスタム条件の作成 API を使用して作成されたので、削除された場合でも、[!DNL Target] UI から削除されません。

おめでとう！ [!DNL Recommendations] API を使用して、カスタム条件の作成、リスト、編集、削除、詳細の取得をおこなうことができるようになりました。 次の節では、[!DNL Target] Delivery API を使用してレコメンデーションを取得します。

[次：「サーバー側配信 API を使用したRecommendationsの取得」>](fetch-recs-server-side-delivery-api.md)
