---
title: 機能のフラグ設定
description: Adobe Targetを使用すれば、カラー、コピー、ボタン、テキスト、画像などの UX 機能を試し、その機能を特定のユーザーに提供できます。
role: Developer
level: Intermediate
topic: Mobile, Personalization
feature: Implement Mobile
doc-type: tutorial
kt: 3040
exl-id: 034d13f2-63b1-44b0-b3dc-867efe37672f
source-git-commit: 342e02562b5296871638c1120114214df6115809
workflow-type: tm+mt
source-wordcount: '732'
ht-degree: 1%

---

# 機能のフラグ設定

モバイルアプリの製品所有者は、複数のアプリリリースに投資することなく、アプリに新機能を展開できる柔軟性を必要としています。 また、効果をテストするために、ユーザーベースの割合で徐々に機能をロールアウトする必要がある場合もあります。 Adobe Targetを使用すれば、カラー、コピー、ボタン、テキスト、画像などの UX 機能を試し、その機能を特定のユーザーに提供できます。

このレッスンでは、特定のアプリ機能を有効にするためのトリガーとして使用できる「機能フラグ」オファーを作成します。

## 学習目標

このレッスンを終了すると、次の操作を実行できるようになります。

* バッチ・プリフェッチ・リクエストに新しい場所を追加します。
* 機能フラグとして使用されるオファーを持つ [!DNL Target] アクティビティを作成します
* アプリでの機能フラグオファーの読み込みと検証

## 「ホーム」アクティビティへのプリフェッチリクエストに新しい場所を追加

前のレッスンのデモアプリでは、「wetravel_feature_flag_recs」という新しい場所をホームアクティビティのプリフェッチリクエストに追加し、新しい Java メソッドで画面に読み込みます。

>[!NOTE]
>
>プリフェッチ・リクエストを使用する利点の 1 つは、新しいリクエストを追加してもネットワーク・オーバーヘッドが追加されず、また、プリフェッチ・リクエスト内にパッケージ化されるため、負荷の追加作業が発生しないことです

まず、wetravel_feature_flag_recs 定数が Constant.java ファイルに追加されていることを確認します。

![&#x200B; フィーチャ フラグ定数を追加 &#x200B;](assets/feature_flag_constant.jpg)

次にコードを示します。

```java
public static final String wetravel_feature_flag_recs = "wetravel_feature_flag_recs";
```

次に、プリフェッチリクエストに場所を追加し、`processFeatureFlags()` という新しい関数を読み込みます。

![&#x200B; 機能フラグコード &#x200B;](assets/feature_flag_code.jpg)

完全に更新されたコードを次に示します。

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

### 機能フラグリクエストの検証

コードを追加したら、ホーム アクティビティでエミュレーターを実行し、更新された応答をログで確認します。

![&#x200B; 機能フラグの場所を検証 &#x200B;](assets/feature_flag_code_logcat.jpg)

## 機能フラグ JSON オファーの作成

次に、特定のオーディエンス（アプリで機能ロールアウトを受け取るオーディエンス）のフラグまたはトリガーとして機能するシンプルな JSON オファーを作成します。 [!DNL Target] インターフェイスで、新しいオファーを作成します。

![&#x200B; 機能フラグ JSON オファーの作成 &#x200B;](assets/feature_flag_json_offer.jpg)

「機能フラグ v1」に値 {&quot;enable&quot;:1} という名前を付けてみましょう

![feature_flag_v1 JSON オファー &#x200B;](assets/feature_flag_json_name.jpg)

## アクティビティの作成

次に、そのオファーを使用して「A/B テスト」アクティビティを作成します。 アクティビティの作成手順について詳しくは、前のレッスンを参照してください。 この例では、アクティビティに必要なオーディエンスは 1 つだけです。 ライブシナリオでは、特定の機能のロールアウト用に特定のカスタムオーディエンスを構築してから、それらのオーディエンスを使用するようにアクティビティを設定できます。 この例では、トラフィックを 50/50 （50% は機能アップデートを表示する訪問者、50% は標準エクスペリエンスを表示する訪問者）に割り当てます。 アクティビティの設定は次のとおりです。

1. アクティビティに「機能フラグ」という名前を付ける
1. 「wetravel_feature_flag_recs」の場所を選択
1. コンテンツを「機能フラグ v1」 JSON オファーに変更します

   ![&#x200B; 機能フラグアクティビティ設定 &#x200B;](assets/feature_flag_activity.jpg)

1. 「**[!UICONTROL Add Experience]**」をクリックしてエクスペリエンス B を追加します。
1. 「wetravel_feature_flag_recs」の場所を離れる
1. コンテンツ **[!UICONTROL Default Content]** は残す
1. **[!UICONTROL Next]** をクリックして [!UICONTROL Targeting] 画面に進みます

   ![&#x200B; 機能フラグアクティビティ設定 &#x200B;](assets/feature_flag_activity_2.jpg)

1. [!UICONTROL Targeting] の画面で、[!UICONTROL Traffic Allocation] メソッドがデフォルト設定（手動）に設定されていること、および各エクスペリエンスにデフォルトの 50% 配分が設定されていることを確認します。 **[!UICONTROL Next]** を選択して **[!UICONTROL Goals & Settings]** に進みます。

   ![&#x200B; 機能フラグアクティビティ設定 &#x200B;](assets/feature_flag_activity_3.jpg)

1. **[!UICONTROL Primary Goal]** を **[!UICONTROL Conversion]** に設定します。
1. アクションを **[!UICONTROL Viewed an Mbox]** に設定します。 「wetravel_context_dest」の場所を使用します（この場所は確認画面にあるので、この場所を使用して、新機能によってより多くのコンバージョンが発生するかどうかを確認できます）。
1. **[!UICONTROL Save & Close]** をクリックします。

   ![&#x200B; 機能フラグアクティビティ設定 &#x200B;](assets/feature_flag_activity_4.jpg)

アクティビティをアクティブ化します。

## 機能フラグアクティビティの検証

次に、エミュレーターを使用してリクエストを監視します。 ターゲティングを 50% のユーザーに設定したので、50% が存在する場合は、機能フラグの応答に `{enable:1}` 値が含まれています。

![&#x200B; 機能フラグの検証 &#x200B;](assets/feature_flag_validation.jpg)

`{enable:1}` の値が表示されない場合は、エクスペリエンスのターゲットになっていません。 一時的なテストとして、オファーを強制的に表示するには、次の操作を行います。

1. アクティビティを非アクティブ化します。
1. 新機能エクスペリエンスでトラフィックの割り当てを 100% に変更します。
1. 保存して再アクティブ化します。
1. エミュレーターのデータを消去してから、アプリを再起動します。
1. オファーは `{enable:1}` 値を返すはずです。

ライブシナリオでは、`{enable:1}` 応答を使用して、ターゲットオーディエンスに表示する特定の機能セットを表示するために、アプリでより多くのカスタムロジックを有効にすることができます。

## まとめ

すばらしい！ これで、特定のユーザーオーディエンスに機能をロールアウトするために必要なスキルを習得しました。
