---
title: Delivery API でRecommendationsを取得する方法
description: チュートリアルのこの部分では、Adobe Target Delivery API を使用して Recommendations コンテンツを取得するために必要な手順を開発者に説明します。
role: Developer
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Recommendations, Administration & Configuration
doc-type: tutorial
kt: 3815
author: Judy Kim
exl-id: 553d1208-647f-479d-acc7-d7760469d642
source-git-commit: 342e02562b5296871638c1120114214df6115809
workflow-type: tm+mt
source-wordcount: '1418'
ht-degree: 2%

---

# Delivery API を使用した [!DNL Recommendations] の取得

Adobe TargetとAdobe Targetの [!DNL Recommendations] API を使用して Web ページにHTMLを配信できますが、アプリ、画面、コンソール、E メール、キオスク、その他のディスプレイデバイスなど、応答を非応答ベースのエクスペリエンスで使用することもできます。 つまり、[!DNL Target] ライブラリと JavaScript を使用できない場合でも、**[!DNL Target]Delivery API** を使用すれば、幅広い [!DNL Target] 機能にアクセスして、パーソナライズされたエクスペリエンスを提供できます。

>[!NOTE]
>
> 実際のレコメンデーション（推奨商品や品目）を含むコンテンツをリクエストする場合は、[!DNL Target] Delivery API を使用します。

レコメンデーションを取得するには、ユーザー ID（最近表示された項目などのプロファイル固有のレコメンデーションで使用）、関連する mbox 名、mbox パラメーター、プロファイルパラメーター、その他の属性を含む適切なコンテキスト情報を使用して、Adobe Target Delivery APIPOST呼び出しを送信します。 応答には、推奨される entity.id が含まれ（他のエンティティデータも含まれる）、JSON 形式またはHTML形式で、デバイスに表示できます。

Adobe Targetの [Delivery API](https://developers.adobetarget.com/api/delivery-api/) は、標準の [!DNL Target] リクエストで提供される既存の機能をすべて公開します。

>[!NOTE]
>Delivery API は次のとおりです。
>* 場所とオーディエンスのエクスペリエンスまたはオファーを RESTful な方法で取得できます。
>* 認証は不要です。
>* POST のみ。
>* Cookie またはリダイレクト呼び出しを処理しません。
>* 「ユーザーの役割」を必要とせず、認識もしません。 単にコンテンツを取得したり、[!DNL Target] エッジサーバーにイベントを報告したりします。


Delivery API を使用して [!DNL Target] エクスペリエンス（レコメンデーションを含む）を配信するには、次の手順に従います。

1. （Visual Experience Composer ではなく）フォームベースのコンポーザーを使用して、[!DNL Target] アクティビティ（A/B、XT、AP、または [!DNL Recommendations]）を作成します。
2. Delivery API を使用して、先ほど作成した [!DNL Target] アクティビティによって生成されたリクエストに対する応答を取得します。

<!-- Q: Why are BOTH steps necessary for this? If you have a Form-based recommendation defined for an mbox, what's the point/benefit of ALSO having the Delivery API step in to retrieve results? Why can't you just have the Form-based Rec deliver the results in the destination device...?? A: See use case below... it's when you want to "intercept" the pending results in order to do more stuff prior to displaying the results. Things like real-time comparisons to inventory levels. -->

## フォームベースの Experience Composer を使用したレコメンデーションの作成

Delivery API で使用できるレコメンデーションを作成するには、[ フォームベースの Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=en) を使用します。

1. 最初に、レコメンデーションで使用する JSON ベースのデザインを作成して保存します。 サンプル JSON、およびフォームベースのアクティビティの設定時に JSON 応答を返す方法に関する背景情報については、[Recommendation Designs の作成 ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html?lang=en) に関するドキュメントを参照してください。 この例では、デザインの名前は *Simple JSON.* です。

   ![server-side-create-recs-json-design.png](assets/server-side-create-recs-json-design.png)

2. [!DNL Target] で、**[!UICONTROL アクティビティ ] / [!UICONTROL  アクティビティを作成 ] / [!UICONTROL Recommendations]** に移動し、「**[!UICONTROL フォーム]**」を選択します。

   ![server-side-create-recs.png](assets/server-side-create-recs.png)

3. プロパティを選択し、[**[!UICONTROL 次へ]**] をクリックします。
4. レコメンデーションの応答を受け取る場所を定義します。 次の例では、*api_charter* という名前の場所を使用しています。 先ほど作成した JSON ベースのデザインを、*Simple JSON.* という名前で選択します。
   ![server-side-create-recs-form.png](assets/server-side-create-recs-form1.png)
5. 保存してレコメンデーションをアクティブ化します。 結果が生成されます。 [結果の準備が整ったら](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-activity/previewing-and-launching-your-recommendations-activity.html?lang=en)、Delivery API を使用して結果を取得できます。

## Delivery API の使用

[Delivery API](https://developers.adobetarget.com/api/delivery-api/#tag/Delivery-API) の構文は次のとおりです。

`POST https://{{CLIENT_CODE}}.tt.omtrdc.net/rest/v1/delivery`

1. クライアントコードは必須です。 クライアントコードは、**[!UICONTROL Recommendations] / [!UICONTROL  設定]** に移動するとAdobe Targetに表示される場合があります。 **[!UICONTROL Recommendation API トークン]** セクションの **[!UICONTROL クライアントコード]** の値に注意してください。
   ![client-code.png](assets/client-code.png)
1. クライアントコードを入手したら、Delivery API 呼び出しを作成します。 次の例は、[Delivery API Postman コレクション ](https://developers.adobetarget.com/api/delivery-api/#section/Getting-Started/Postman-Collection) で提供される **[!UICONTROL Web バッチ処理された mbox Delivery API Call]** で始まり、関連する変更をおこないます。 次に例を示します。
   * **browser** および **address** オブジェクトは、非HTMLの使用例では必要ないので、 **Body** から削除されました。
   * *この例で* は、ロケーション名として api_charter が表示されます。
   * entity.id を指定します。これは、このレコメンデーションがコンテンツの類似性に基づいているためで、現在の項目キーを [!DNL Target] に渡す必要があるからです。
      ![server-side-delivery-API-call.png クエリパラメ](assets/server-side-delivery-api-call2.png)
ーターを正しく設定するようにしてください。例えば、必ず 
`{{CLIENT_CODE}}` 必要に応じて。<!--Q: In the updated call syntax, entity.id is listed as a profileParameter instead of an mboxParameter as in older versions. --> <!--Q: Old image ![server-side-create-recs-post.png](assets/server-side-create-recs-post.png) Old accompanying text: "Note this recommendation is based on Content Similar products based on the entity.id sent via mboxParameters." -->
      ![client-code3](assets/client-code3.png)
1. リクエストを送信します。 これは、*api_charter* の場所に対して実行されます。この場所では、アクティブなレコメンデーションが実行され、JSON デザインで定義され、レコメンデーションされたエンティティのリストが出力されます。
1. JSON デザインに基づいて応答を受信します。
   ![server-side-create-recs-json-response2.](assets/server-side-create-recs-json-response2.png)
png 応答には、キー ID と、推奨エンティティのエンティティ ID が含まれます。

この方法で Delivery API を [!DNL Recommendations] と共に使用すると、HTML以外のデバイスで訪問者にレコメンデーションを表示する前に、追加の手順を実行できます。 例えば、Delivery API からの応答を利用して、最終結果を表示する前に、別のシステム（CMS、PIM、e コマースプラットフォームなど）からエンティティ属性の詳細（在庫、価格、評価など）をリアルタイムで追加検索できます。

このチュートリアルで概要を説明したアプローチを使用すると、[!DNL Target] からの応答を活用してパーソナライズされたレコメンデーションを提供する任意のアプリケーションを入手できます。

## 実装例

以下のリソースは、非HTML実装の様々な例です。 システムとデバイスが関係するので、すべての実装は一意であることに注意してください。

| リソース | 詳細 |
| --- | --- |
| [Adobe Target Everywhere - IoT でサーバー側またはを実装](https://expleague.azureedge.net/labs/L733/index.html) | Adobe Targetのサーバー側 API を利用した React アプリケーションに関する実践的な操作を提供するAdobe Summit2019 Lab。 |
| [AdobeSDK を使用しないモバイルアプリでのAdobe Target](https://community.tealiumiq.com/t5/Universal-Data-Hub/Adobe-Target-in-a-Mobile-App-Without-the-Adobe-SDK/ta-p/26753) | このガイドでは、AdobeSDK をインストールせずにモバイルアプリでAdobe Targetを設定する方法を説明します。 このソリューションでは、Tealium SDK の Web ビューとリモートコマンドモジュールを使用して、Adobe訪問者 API(Experience Cloud) とAdobe Target API にリクエストを送受信します。 |
| [モバイルアプリにおけるAdobe Targetの仕組み](https://experienceleague.adobe.com/docs/target/using/implement-target/mobile-apps/mobile-how-target-works-mobile-apps.html?lang=en) | [!DNL Target] が Mobile SDK でどのように動作するか |
| [API の [!DNL Target] extension in Experience Platform Launch and Implementing [!DNL Target] 設定](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/adobe-target) | Experience Platform Launchで [!DNL Target] 拡張機能を設定し、アプリに [!DNL Target] 拡張機能を追加し、[!DNL Target] API を実装してアクティビティをリクエストし、オファーをプリフェッチし、ビジュアルプレビューモードを開始する手順。 |
| [Adobe Target Node Client](https://www.npmjs.com/package/@adobe/target-nodejs-sdk) | オープンソースの [!DNL Target] Node.js SDK v1.0 |
| [サーバー側の概要](https://experienceleague.adobe.com/docs/target/using/implement-target/server-side/api-and-sdk-overview.html?lang=en) | Adobe Target Server Side Delivery API、Server Side Batch Delivery API、Node.js SDK、Adobe Target [!DNL Recommendations] API に関する情報です。 |
| [Adobe Campaign Content Recommendations in Email](https://medium.com/adobetech/adobe-campaign-content-recommendations-in-email-b51ced771d7f) | Adobe CampaignのAdobe TargetとAdobe I/O Runtimeを介して E メールでコンテンツのレコメンデーションを活用する方法を説明するブログ。 |

## API を使用した [!DNL Recommendations] 設定の管理

ほとんどの場合、レコメンデーションはAdobe Target UI で設定され、前述の節で説明したような理由で、 [!DNL Target] API を介して使用またはアクセスされます。 この UI と API の調整は一般的です。 ただし、セットアップと結果の使用の両方の API を介してすべてのアクションを実行したい場合があります。 一般的ではありませんが、ユーザーは絶対に設定、実行、*および* をおこなえば、レコメンデーションの結果を API を使用して完全に活用できます。

Adobe Target Recommendationsエンティティを管理し、サーバー側で配信する方法について、[ 前の節 ](manage-catalog.md) で学びました。 同様に、Adobe I/Oを使用すると、Adobe Targetにログインしなくても、条件、プロモーション、コレクション、デザインテンプレートを管理できます。 すべての [!DNL Recommendations] API の完全なリストは、[ ここ ](https://developers.adobetarget.com/api/recommendations/) にありますが、参照用の要約を示します。

| リソース | 詳細 |
| --- | --- |
| [コレクション](https://developers.adobetarget.com/api/recommendations/#tag/Collections) | コレクションのリスト、作成、取得、編集、削除を行います。 |
| [条件](https://developers.adobetarget.com/api/recommendations/#tag/Criteria) | 条件をリストして取得します。 |
| [デザイン](https://developers.adobetarget.com/api/recommendations/#tag/Designs) | デザインのリスト、作成、取得、編集、削除、検証を行います。 |
| [エンティティ](https://developers.adobetarget.com/api/recommendations/#tag/Entities) | エンティティを保存、削除、取得します。 |
| [プロモーション](https://developers.adobetarget.com/api/recommendations/#tag/Promotions) | プロモーションのリスト、作成、取得、編集、削除を行います。 |
| [カテゴリ条件](https://developers.adobetarget.com/api/recommendations/#tag/Category-Criteria) | カテゴリ条件のリスト、作成、取得、編集、削除を行います。 |
| [カスタム条件](https://developers.adobetarget.com/api/recommendations/#tag/Custom-Criteria) | カスタム条件のリスト、作成、取得、編集および削除を行います。 |
| [品目基準](https://developers.adobetarget.com/api/recommendations/#tag/Item-Criteria) | 項目基準のリスト、作成、取得、編集、削除を行います。 |
| [人気度の条件](https://developers.adobetarget.com/api/recommendations/#tag/Popularity-Criteria) | 人気度条件のリスト、作成、取得、編集および削除。 |
| [プロファイル属性条件](https://developers.adobetarget.com/api/recommendations/#tag/Profile-Attribute-Criteria) | プロファイル属性条件のリスト、作成、取得、編集、削除を行います。 |
| [最近の条件](https://developers.adobetarget.com/api/recommendations/#tag/Recent-Criteria) | 最近の条件のリスト、作成、取得、編集、削除を行います。 |
| [シーケンス条件](https://developers.adobetarget.com/api/recommendations/#tag/Sequence-Criteria) | シーケンス条件のリスト、作成、取得、編集、削除を行います。 |

## リファレンスドキュメント

* [Adobe Target API ドキュメント](https://developers.adobetarget.com/api/#getting-started)
* [Adobe Target Delivery API](https://developers.adobetarget.com/api/delivery-api/)
* [ [!DNL Recommendations] 電子メールとの統合](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/integrating-recs-email.html?lang=en)

## 概要とレビュー

おめでとう！ このチュートリアルを終了して、次の方法を学習しました。
* [Recommendations API を使用したカタログの管理](manage-catalog.md)
* [Recommendations API を使用したカスタム条件の管理](manage-custom-criteria.md)
* [Recommendationsでの Delivery API の使用](fetch-recs-server-side-delivery-api.md)
