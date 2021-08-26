---
title: リクエストへのパラメーターの追加
description: このレッスンでは、前のレッスンで追加したTargetリクエストに、Adobeのライフサイクル指標とカスタムパラメーターを追加します。 これらの指標およびパラメーターは、後のチュートリアルで、パーソナライズされたオーディエンスを作成するために使用されます。
role: Developer
level: Intermediate
topic: Mobile, Personalization
feature: Implement Mobile
doc-type: tutorial
kt: 3040
thumbnail: null
exl-id: 0250e55f-a233-4060-84e1-86d1f88a6106
source-git-commit: a6b645b6d9693a4c8882fd47ee0d61698c0b834d
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 0%

---

# リクエストへのパラメーターの追加

このレッスンでは、前のレッスンで追加した[!DNL Target]リクエストに、Adobeのライフサイクル指標とカスタムパラメーターを追加します。 これらの指標およびパラメーターは、後のチュートリアルで、パーソナライズされたオーディエンスを作成するために使用されます。

## 学習内容

このレッスンを最後まで学習すると、次の内容を習得できます。

* モバイルライフサイクルAdobeの追加
* プリフェッチ要求へのパラメーターの追加
* ライブ場所へのパラメーターの追加
* 両方のリクエストのパラメーターの検証

## ライフサイクルパラメーターの追加

[Adobeモバイルライフサイクル指標](https://docs.adobe.com/content/help/en/mobile-services/android/metrics.html)を有効にします。 これにより、ユーザーのデバイスやアプリの使用に関する豊富な情報を含む場所のリクエストにパラメーターが追加されます。 次のレッスンでは、ライフサイクルリクエストが提供するデータを使用してオーディエンスを構築します。

ライフサイクル指標を有効にするには、HomeActivityコントローラを再度開き、onResume()関数に`Config.collectLifecycleData(this);`を追加します。

![ライフサイクルリクエスト](assets/lifecycle_code.jpg)

### プリフェッチ要求のライフサイクルパラメータの検証

エミュレーターを実行し、Logcatを使用してライフサイクルパラメーターを検証します。 「プリフェッチ」のフィルターを使用してプリフェッチ応答を検索し、新しいパラメーターを探します。
![ライフサイクル検証](assets/lifecycle_validation.jpg)

`Config.collectLifecycleData()`をHomeActivityコントローラに追加しただけですが、Targetリクエストと共に送信されるライフサイクル指標もThankYou画面に表示されます。

## プリフェッチ要求へのat_propertyパラメーターの追加

Adobe Targetのプロパティは[!DNL Target]インターフェイスで定義され、アプリやWebサイトのパーソナライズに関する境界を確立するために使用されます。 at_propertyパラメーターは、オファーやアクティビティがアクセスおよび管理される特定のプロパティを識別します。 プリフェッチおよびライブロケーションのリクエストにプロパティを追加します。

>[!NOTE]
>
>ライセンスによっては、 [!DNL Target]インターフェイスに「プロパティ」オプションが表示される場合と表示されない場合があります。 これらのオプションがない場合や、会社でプロパティを使用していない場合は、このレッスンの次の節に進んでください。

at_propertyの値は、[!UICONTROL セットアップ] / [!UICONTROL プロパティ]の下の[!DNL Target]インターフェイスで取得できます。  プロパティの上にマウスポインターを置き、コードスニペットアイコンを選択して、`at_property`値をコピーします。

![at_propertyをコピーする](assets/at_property_interface.jpg)

次のように、プリフェッチリクエストの各ロケーションのパラメーターとして追加します。
![at_propertyパラメーター](assets/params_at_property.jpg)を追加します
以下に、`targetPrefetchContent()`関数の更新後のコードを示します（at_propertyの値を&#x200B;]_プレースホルダーテキストを_[!UICONTROL &#x200B;に更新してください）。

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

今後のプロジェクトでは、追加のパラメーターを実装する必要が生じる場合があります。 `createTargetPrefetchObject()`メソッドでは、次の3種類のパラメーターを使用できます。`locationParams`、`orderParams`、および`productParams`。 プリフェッチ要求](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/c-mob-target-prefetch-android.html?lang=en)にこれらのパラメーターを追加する方法の詳細については、[のドキュメントを参照してください。

また、プリフェッチ要求の各場所に異なるロケーションパラメーターを追加できます。 たとえば、param2という別のマップを作成し、新しいパラメータを配置して、ある場所にparam2を設定し、別の場所にparam1を設定します。 次に例を示します。

```java
prefetchList.add(Target.createTargetPrefetchObject(location1_name, params1);
prefetchList.add(Target.createTargetPrefetchObject(location2_name, params2);
```

## プリフェッチ要求でのat_propertyパラメーターの検証

次に、エミュレーターを実行し、Logcatを使用して、両方の場所のプリフェッチ要求と応答でat_propertyが表示されていることを確認します。
![at_propertyパラメーター](assets/parameters_at_property_validation.jpg)を検証します。

## ライブロケーションリクエストへのカスタムパラメーターの追加

ライブロケーションリクエスト(wetravel_context_dest)は、前のレッスンで追加され、予約プロセスの最終確認画面に関連するプロモーションを表示できるようになりました。 ユーザーの宛先に基づいてプロモーションをパーソナライズし、それをリクエストのパラメーターとして追加します。 また、トロップの原点とat_propertyの値に対するパラメーターも追加します。

ThankYouActivityコントローラーのtargetLoadRequest()関数に次のパラメーターを追加します。
![ライブロケーションリクエストにパラメーターを追加](assets/parameters_live_location.jpg)
以下に、 targetLoadRequest()関数の更新されたコードを示します（「ここにat_property値を追加」プレースホルダーテキストを必ず更新してください）。

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

エミュレーターを実行し、Logcatを開きます。 いずれかのパラメーターをフィルターして、リクエストに必要なパラメーターが含まれていることを確認します。
![ライブロケーションリクエストのカスタムパラメーターの検証](assets/parameters_live_location_validation.jpg)

>[!NOTE]
>
>注文確認要求およびパラメーター：このデモプロジェクトでは使用しませんが、注文の詳細は通常、実際の実装でキャプチャされるので、[!DNL Target]は注文の詳細を指標/ディメンションとして使用できます。 [注文確認要求とパラメーター](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/c-target-methods.html?lang=en)の実装方法については、ドキュメントを参照してください。

>[!NOTE]
>
>Analytics for Target(A4T):Adobe Analyticsは、[!DNL Target]のレポートソースとして設定できます。 これにより、Target SDKが収集したすべての指標/ディメンションをAdobe Analyticsで表示できます。 詳しくは、「[A4Tの概要](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=en)」を参照してください。

お疲れさまでした。 パラメーターが設定されたので、これらのパラメーターを使用してAdobe Targetでオーディエンスとオファーを作成する準備が整いました。

**[次へ：「オーディエンスとオファーの作成」>](create-audiences-and-offers.md)**
