---
title: リクエストへのパラメーターの追加
description: このレッスンでは、前のレッスンで追加した Target リクエストに、Adobeのライフサイクル指標とカスタムパラメーターを追加します。 これらの指標およびパラメーターは、後のチュートリアルで、パーソナライズされたオーディエンスを作成する際に使用されます。
role: Developer
level: Intermediate
topic: Mobile, Personalization
feature: Implement Mobile
doc-type: tutorial
kt: 3040
exl-id: 0250e55f-a233-4060-84e1-86d1f88a6106
source-git-commit: 342e02562b5296871638c1120114214df6115809
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 0%

---

# リクエストへのパラメーターの追加

このレッスンでは、前のレッスンで追加した [!DNL Target] リクエストに、Adobeのライフサイクル指標とカスタムパラメーターを追加します。 これらの指標およびパラメーターは、後のチュートリアルで、パーソナライズされたオーディエンスを作成する際に使用されます。

## 学習内容

このレッスンを最後まで学習すると、次の内容を習得できます。

* モバイルライフサイクルAdobe指標の追加
* プリフェッチ要求へのパラメーターの追加
* ライブの場所へのパラメーターの追加
* 両方のリクエストのパラメーターの検証

## ライフサイクルパラメータの追加

[ モバイルライフサイクル指標 ](https://experienceleague.adobe.com/docs/mobile-services/android/metrics.html?lang=en) のAdobeを有効にします。 これにより、ユーザーのデバイスやアプリとの関与に関する豊富な情報を含む場所のリクエストにパラメーターが追加されます。 次のレッスンでは、ライフサイクルリクエストが提供するデータを使用してオーディエンスを構築します。

ライフサイクル指標を有効にするには、HomeActivity コントローラを再度開き、onResume() 関数に `Config.collectLifecycleData(this);` を追加します。

![ライフサイクルリクエスト](assets/lifecycle_code.jpg)

### プリフェッチ要求のライフサイクルパラメータの検証

エミュレーターを実行し、Logcat を使用してライフサイクルパラメーターを検証します。 プリフェッチ応答を検索し、新しいパラメータを探す「プリフェッチ」のフィルタ：
![ ライフサイクル検証 ](assets/lifecycle_validation.jpg)

`Config.collectLifecycleData()` を HomeActivity コントローラに追加しただけですが、Target リクエストと共に送信されたライフサイクル指標も ThankYou 画面に表示されます。

## プリフェッチ要求への at_property パラメーターの追加

Adobe Targetのプロパティは [!DNL Target] インターフェイスで定義され、アプリや Web サイトをパーソナライズする際の境界を確立するために使用されます。 at_property パラメーターは、オファーやアクティビティにアクセスして管理する特定のプロパティを識別します。 プリフェッチおよびライブロケーションのリクエストにプロパティを追加します。

>[!NOTE]
>
>ライセンスによっては、 [!DNL Target] インターフェイスに「プロパティ」オプションが表示される場合と表示されない場合があります。 これらのオプションがない場合や、会社でプロパティを使用していない場合は、このレッスンの次の節に進んでください。

at_property の値は、[!UICONTROL  セットアップ ] / [!UICONTROL  プロパティ ] の下の [!DNL Target] インターフェイスで取得できます。  プロパティの上にマウスポインターを置き、コードスニペットアイコンを選択して、`at_property` 値をコピーします。

![at_property をコピーする](assets/at_property_interface.jpg)

次のように、プリフェッチ要求内の各場所のパラメーターとして追加します。
![at_property パラメーター ](assets/params_at_property.jpg) を追加します
`targetPrefetchContent()` 関数の更新後のコードを次に示します（_[!UICONTROL at_property 値は]_ プレースホルダーテキストに置き換えてください）。

```java
public void targetPrefetchContent() {
        List<TargetPrefetchObject> prefetchList = new ArrayList<>();

        Map<String, Object> params1;
        params1 = new HashMap<String, Object>();
        params1.put("at_property", "your at_property value goes here");

        prefetchList.add(Target.createTargetPrefetchObject(Constant.wetravel_engage_home, params1));
        prefetchList.add(Target.createTargetPrefetchObject(Constant.wetravel_engage_search, params1));
        Target.TargetCallback<Boolean> prefetchStatusCallback = new Target.TargetCallback<Boolean>() {
            @Override
            public void call(final Boolean status) {
                HomeActivity.this.runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        String cachingStatus = status ? "YES" : "NO";
                        System.out.println("Received Response from prefetch : " + cachingStatus);
                        engageMessage();
                        setUp();

                    }
                });
            }};
        Target.prefetchContent(prefetchList, null, prefetchStatusCallback);
    }
```

### パラメーターに関する注意

将来のプロジェクトでは、追加のパラメーターを実装する必要が生じる場合があります。 `createTargetPrefetchObject()` メソッドでは、次の 3 種類のパラメーターを使用できます。`locationParams`、`orderParams`、および `productParams`。 プリフェッチ要求 ](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/c-mob-target-prefetch-android.html?lang=en) にこれらのパラメーターを追加する方法の詳細については、[ ドキュメントを参照してください。

また、プリフェッチ要求の各場所に異なる場所パラメーターを追加できます。 たとえば、param2 という別のマップを作成し、新しいパラメータを配置して、ある場所に param2 を設定し、別の場所に param1 を設定することができます。 次に例を示します。

```java
prefetchList.add(Target.createTargetPrefetchObject(location1_name, params1);
prefetchList.add(Target.createTargetPrefetchObject(location2_name, params2);
```

## プリフェッチ要求での at_property パラメーターの検証

次に、エミュレーターを実行し、Logcat を使用して、プリフェッチ要求と応答で at_property が表示されていることを確認します。
![at_property パラメーター ](assets/parameters_at_property_validation.jpg) を検証します。

## ライブ場所の要求へのカスタムパラメーターの追加

前のレッスンでライブロケーションリクエスト (wetravel_context_dest) が追加され、予約プロセスの最終確認画面に関連するプロモーションを表示できるようになりました。 ユーザーの宛先に基づいてプロモーションをパーソナライズし、それをリクエストのパラメーターとして追加します。 また、トロップの原点と at_property 値のパラメータも追加します。

ThankYouActivity コントローラーの targetLoadRequest() 関数に次のパラメーターを追加します。
![ ライブロケーションリクエストにパラメーターを追加 ](assets/parameters_live_location.jpg)
以下に、targetLoadRequest() 関数の更新後のコードを示します（「at_property value here」プレースホルダーテキストを必ず更新してください）。

```java
public void targetLoadRequest(final ArrayList<Recommandation> recommandations) {
    Map<String, Object> locationParams = new HashMap<>();
    locationParams.put("at_property","add your at_property value here");
    locationParams.put("locationSrc", (""+Utility.getInSharedPreference(ThankYouActivity.this,Constant.departure,"")));
    locationParams.put("locationDest", (""+Utility.getInSharedPreference(ThankYouActivity.this,Constant.destination,"")));

    Target.loadRequest(Constant.wetravel_context_dest, "", null, null, locationParams, new Target.TargetCallback<String>() {
        @Override
        public void call(final String response) {
        try {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    AppDialogs.dialogLoaderHide();
                    filterRecommendationBasedOnOffer(recommandations, response);
                    recommandationbAdapter.notifyDataSetChanged();
                }
            });
        } catch (Exception e) {
            e.printStackTrace();
        }
        }
    });
    Target.clearPrefetchCache();
}
```

### ライブロケーションリクエストのカスタムパラメーターの検証

エミュレーターを実行し、Logcat を開きます。 いずれかのパラメーターをフィルターして、要求に必要なパラメーターが含まれていることを確認します。
![ ライブロケーションリクエストのカスタムパラメーターの検証 ](assets/parameters_live_location_validation.jpg)

>[!NOTE]
>
>注文確認要求およびパラメーター：このデモプロジェクトでは使用しませんが、注文の詳細は通常、実際の実装でキャプチャされるので、[!DNL Target] は注文の詳細を指標/ディメンションとして使用できます。 [ 注文確認要求とパラメーター ](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/c-target-methods.html?lang=en) の実装方法については、ドキュメントを参照してください。

>[!NOTE]
>
>Analytics for Target(A4T):Adobe Analyticsは、[!DNL Target] のレポートソースとして設定できます。 これにより、Target SDK が収集したすべての指標/ディメンションをAdobe Analyticsで表示できます。 詳しくは、[A4T の概要 ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=en) を参照してください。

お疲れさま！ パラメーターが設定されたので、これらのパラメーターを使用して、Adobe Targetでオーディエンスとオファーを作成する準備が整いました。

**[次へ：「オーディエンスとオファーの作成」>](create-audiences-and-offers.md)**
