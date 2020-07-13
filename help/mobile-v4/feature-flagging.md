---
title: 機能のフラグ付け
seo-title: 機能のフラグ付け
description: Adobe Targetは、色、コピー、ボタン、テキスト、イメージなどのUX機能を試して、それらの機能を特定のオーディエンスに提供するのに使用できます。
seo-description: Adobe Targetは、色、コピー、ボタン、テキスト、イメージなどのUX機能を試して、それらの機能を特定のオーディエンスに提供するのに使用できます。
feature: mobile
kt: 3040
audience: developer
doc-type: tutorial
activity-type: implement
translation-type: tm+mt
source-git-commit: 199fbde58696a0511623c5500cc6afbbcfdd67a3
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 1%

---


# 機能のフラグ付け

モバイルアプリの製品所有者は、複数のアプリのリリースに投資することなく、アプリの新機能を柔軟に展開できる必要があります。 また、効果をテストするために、機能をユーザーベースの一定の割合まで徐々にロールアウトしたい場合もあります。 Adobe Targetは、色、コピー、ボタン、テキスト、イメージなどのUX機能を試して、それらの機能を特定のオーディエンスに提供するのに使用できます。

このレッスンでは、特定のアプリ機能を有効にするトリガーとして使用できる「機能フラグ」オファーを作成します。

## 学習目標

このレッスンを終了すると、次のことができます。

* バッチプリフェッチ要求追加の新しい場所
* 機能フラグとして使用するオファーを含む [!DNL Target] アクティビティを作成する
* アプリに機能フラグオファーを読み込んで検証する

## ホーム・アクティビティ追加へのプリフェッチ要求の新しい場所

前回のレッスンで説明したデモアプリでは、「wetravel_feature_flag_recs」という新しい場所をホームアクティビティのプリフェッチ要求に追加し、新しいJavaメソッドを使用して画面に読み込みます。

>[!NOTE]
>
>プリフェッチ要求を使用する利点の1つは、新しい要求を追加しても、追加のネットワークオーバーヘッドが発生せず、また、要求がプリフェッチ要求内にパッケージ化されるため、負荷が増えることがないことです

まず、wetravel_feature_flag_recs定数がConstant.javaファイルに追加されていることを確認します。

![追加機能フラグ定数](assets/feature_flag_constant.jpg)

次のコードを示します。

```java
public static final String wetravel_feature_flag_recs = "wetravel_feature_flag_recs";
```

プリフェッチ要求に場所を追加し、次の新しい関数をロードしま `processFeatureFlags()`す。

![機能フラグコード](assets/feature_flag_code.jpg)

以下に、完全な更新コードを示します。

```java
public void targetPrefetchContent() {
    List<TargetPrefetchObject> prefetchList = new ArrayList<>();

    Map<String, Object> params1;
    params1 = new HashMap<String, Object>();
    params1.put("at_property", "7962ac68-17db-1579-408f-9556feccb477");

    prefetchList.add(Target.createTargetPrefetchObject(Constant.wetravel_engage_home, params1));
    prefetchList.add(Target.createTargetPrefetchObject(Constant.wetravel_engage_search, params1));
    prefetchList.add(Target.createTargetPrefetchObject(Constant.wetravel_feature_flag_recs, params1));

    Target.TargetCallback<Boolean> prefetchStatusCallback = new Target.TargetCallback<Boolean>() {
        @Override
        public void call(final Boolean status) {
            HomeActivity.this.runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    String cachingStatus = status ? "YES" : "NO";
                    System.out.println("Received Response from prefetch : " + cachingStatus);
                    engageMessage();
                    processFeatureFlags();
                    setUp();

                }
            });
        }};
    Target.prefetchContent(prefetchList, null, prefetchStatusCallback);
}

public void processFeatureFlags() {
    Target.loadRequest(Constant.wetravel_feature_flag_recs, "", null, null, null,
            new Target.TargetCallback<String>(){
                @Override
                public void call(final String s) {
                    runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            System.out.println("Feature Flags : " + s);
                            if(s != null && !s.isEmpty()) {
                                //enable or disable features
                            }
                        }
                    });
                }
            });
}
```

### 機能フラグの要求の検証

コードが追加されたら、ホームアクティビティでエミュレータを実行し、Logcatで更新された応答を確認します。

![機能フラグの場所の検証](assets/feature_flag_code_logcat.jpg)

## 機能フラグJSONオファーの作成

次に、特定のオーディエンス(アプリで機能のロールアウトを受け取るオーディエンス)のフラグまたはトリガーとして機能する単純なJSONオファーを作成します。 インター [!DNL Target] フェイスで、新しいオファーを作成します。

![機能フラグJSONオファーを作成](assets/feature_flag_json_offer.jpg)

「機能フラグv1」に値{&quot;enable&quot;:1}を付けます。

![feature_flag_v1 JSONオファー](assets/feature_flag_json_name.jpg)

## アクティビティの作成

次に、そのオファーを使用してA/Bテストアクティビティを作成します。 アクティビティの作成に関する詳細な手順については、前のレッスンを参照してください。 この例では、アクティビティに必要なオーディエンスは1つだけです。 ライブシナリオでは、特定の機能のロールアウトに対して特定のカスタムオーディエンスを作成し、それらのオーディエンスを使用するようにアクティビティを設定します。 この例では、トラフィックを50/50(機能の更新を表示する訪問者に50%、標準のエクスペリエンスを表示する訪問者に50%)配分します。 アクティビティの設定を次に示します。

1. アクティビティに「機能フラグ」という名前を付けます。
1. 「wetravel_feature_flag_recs」の場所を選択します
1. コンテンツを「機能フラグv1」JSONオファーに変更します。

   ![機能フラグアクティビティ設定](assets/feature_flag_activity.jpg)

1. 「 **[!UICONTROL 追加エクスペリエンス]** 」をクリックして、エクスペリエンスBを追加します。
1. 「wetravel_feature_flag_recs」の場所をそのままにします。
1. コンテンツ **[!UICONTROL のデフォルトコンテンツ]** をそのまま使用
1. Click **[!UICONTROL Next]** to advance to the [!UICONTROL Targeting] screen

   ![機能フラグアクティビティ設定](assets/feature_flag_activity_2.jpg)

1. 「 [!UICONTROL ターゲット設定] 」画面で、「 [!UICONTROL トラフィック配分] 」の方法がデフォルトの設定（手動）に設定され、各エクスペリエンスのデフォルトの50%の配分が設定されていることを確認します。 「 **[!UICONTROL 次へ]** 」を選択して **[!UICONTROL 目標と設定に進みます]**。

   ![機能フラグアクティビティ設定](assets/feature_flag_activity_3.jpg)

1. 「 **[!UICONTROL プライマリ目標]** 」を「コンバージョン **[!UICONTROL 」に設定します]**。
1. アクションを「mboxを **[!UICONTROL 表示した」に設定します]**。 「wetravel_context_dest」の場所を使用します（この場所は確認画面にあるので、新機能によってコンバージョンが増えたかどうかを調べるのに使用できます）。
1. 「**[!UICONTROL 保存して閉じる]**」をクリックします。

   ![機能フラグアクティビティ設定](assets/feature_flag_activity_4.jpg)

アクティビティをアクティブ化します。

## 機能フラグアクティビティの検証

エミュレータを使用して要求を監視します。 ターゲットをユーザーの50%に設定したので、機能フラグの応答に `{enable:1}` 値が含まれているのが50%となります。

![機能フラグの検証](assets/feature_flag_validation.jpg)

値が表示されない場合は、エクスペリエンスのターゲット設定 `{enable:1}` が解除されていることを意味します。 一時的なテストとして、オファーに表示を強制するには、次の操作を行います。

1. アクティビティを非アクティブ化します。
1. 新機能のエクスペリエンスで、トラフィックの配分を100%に変更します。
1. 保存して再開します。
1. エミュレーター上のデータを消去し、アプリケーションを再起動します。
1. これで、オファーは `{enable:1}` 値を返します。

実稼動中のシナリオでは、 `{enable:1}` 応答を使用して、ターゲットオーディエンスを表示する特定の機能セットを表示するための、アプリ内のより多くのカスタムロジックを有効にできます。

## まとめ

お疲れさま！ これで、特定のユーザオーディエンスに機能を展開するのに必要なスキルが得られました。
