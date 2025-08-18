---
title: Adobe Target リクエストの追加
description: Adobe Mobile Services SDK（v4）は、Adobe Targetの手法と機能を提供し、ユーザーごとに異なるエクスペリエンスでアプリをパーソナライズできるようにします。
role: Developer
level: Intermediate
topic: Mobile, Personalization
feature: Implement Mobile
doc-type: tutorial
kt: 3040
exl-id: 88a5be3f-d61f-43e7-997a-574ef56122ed
source-git-commit: 342e02562b5296871638c1120114214df6115809
workflow-type: tm+mt
source-wordcount: '1785'
ht-degree: 0%

---

# Adobe Target リクエストの追加

Adobe Mobile Services SDK（v4）は、ユーザーごとに異なるエクスペリエンスを使用してアプリをパーソナライズできるAdobe Targetの手法と機能を提供します。 通常、パーソナライズされたコンテンツを取得し、そのコンテンツの影響を測定するために、アプリからAdobe Targetに 1 つ以上のリクエストが行われます。

このレッスンでは、[!DNL Target] リクエストを実装して、パーソナライゼーション用の We.Travel アプリを準備します。

## 前提条件

必ず [ サンプルアプリをダウンロードして更新 ](download-and-update-the-sample-app.md) してください。

## 学習目標

このレッスンを終了すると、次の操作を実行できるようになります。

* バッチプリフェッチリクエストを使用して複数の [!DNL Target] オファー（パーソナライズされたコンテンツ）をキャッシュする
* プリフェッチされた [!DNL Target] の場所の読み込み
* リアルタイムでの [!DNL Target] の場所の読み込み（プリフェッチなし）
* プリフェッチされた場所をキャッシュからクリア
* プリフェッチされたリクエストとリアルタイムリクエストの検証

## 用語

このチュートリアルの残りの部分で使用する、主な Target 用語の一部を以下に示します。

* **リクエスト：** Adobe Target サーバーへのネットワークリクエスト
* **オファー：**&#x200B;[!DNL Target] ユーザーインターフェイス（または API を使用）で定義され、応答で配信される、コードまたはその他のテキストベースのコンテンツのスニペット。 通常、ネイティブモバイルアプリで [!DNL Target] を使用する場合は JSON です。
* **Location:** リクエストに付けられるユーザー定義名。オファーを特定のリクエストに関連付けるために [!DNL Target] インターフェイスで使用されます。
* **バッチリクエスト：** 複数の場所を含む単一のリクエスト
* **プリフェッチリクエスト**：オファーを取得し、アプリで今後使用するためにメモリにキャッシュする単一のリクエスト
* **バッチプリフェッチリクエスト：** 複数の場所のオファーをプリフェッチする単一のリクエスト
* **オーディエンス：**&#x200B;[!DNL Target] インターフェイスで定義された訪問者、または他のAdobe アプリケーションから [!DNL Target] に共有された訪問者のグループ（「iPhone X の訪問者」、「カリフォルニアの訪問者」、「最初のアプリオープン」など）
* **アクティビティ：** [!DNL Target] ユーザーインターフェイス（または API を使用）で定義される [!DNL Target] ーディエンス構成で、場所、オファー、オーディエンスをリンクし、パーソナライズされたエクスペリエンスを作成します

## バッチ・プリフェッチ・リクエストの追加

We.Travel で最初に実装するリクエストは、ホーム画面の 2 つの [!DNL Target] の場所を持つバッチプリフェッチリクエストです。 後のレッスンでは、これらの場所にオファーを設定します。このオファーでは、新規ユーザーが予約プロセスを進める際に役立つメッセージを表示します。

プリフェッチリクエストは [!DNL Target]Adobe Target サーバー応答（オファー）をキャッシュすることで、最小限のコンテンツを取得します。 バッチ事前読み込みリクエストは、それぞれ異なる場所に関連付けられた複数のオファーを取得してキャッシュします。 プリフェッチされたすべての場所は、ユーザー・セッションで後で使用するためにデバイス上にキャッシュされます。 ホーム画面で複数の場所をプリフェッチすることで、訪問者がアプリ内を移動したときに後で使用するオファーを取得できます。 プリフェッチ・メソッドの詳細は、[prefetch ドキュメント ](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/c-mob-target-prefetch-android.html?lang=ja) を参照してください。

### バッチ・プリフェッチ・リクエストの追加

app/main/java/com.wetravel/Controller の下にある HomeActivity コントローラー（ホーム画面のソースコード）を更新しましょう。 赤で表示されている 2 つのコードブロックを追加します。

まず、HomeActivity コントローラー（ホーム画面のソースコード）を使用します。このコントローラーは、app/main/java/com.wetravel/Controller にあります。

赤で表示されている 2 つのコードブロックを追加します。

![HomeActivity プリフェッチ コード ](assets/homeactivity.jpg)

HomeActivity のコードの末尾まで下にスクロールし、`setHeader()` 関数および現在の *関数の* 置き換え `onResume()` の後に以下のコードを追加します。

```java
@Override
protected void onResume() {
    super.onResume();
    targetPrefetchContent();
}

public void targetPrefetchContent() {
    List<TargetPrefetchObject> prefetchList = new ArrayList<>();
    prefetchList.add(Target.createTargetPrefetchObject(Constant.wetravel_engage_home, null));
    prefetchList.add(Target.createTargetPrefetchObject(Constant.wetravel_engage_search, null));
    Target.TargetCallback<Boolean> prefetchStatusCallback = new Target.TargetCallback<Boolean>() {
        @Override
        public void call(final Boolean status) {
            HomeActivity.this.runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    String cachingStatus = status ? "YES" : "NO";
                    System.out.println("Received Response from prefetch : " + cachingStatus);
                    setUp();

                }
            });
        }};
    Target.prefetchContent(prefetchList, null, prefetchStatusCallback);
}
```

おそらく、ファイルに [!DNL Target] クラスが読み込まれていないという警告が表示されます。 次の図に示すように、HomeActivity コントローラの最上部に [!DNL Target] クラスをインポートしてください。

```java
import com.adobe.mobile.Target;
import com.adobe.mobile.TargetPrefetchObject;
```

![Target クラスの読み込み ](assets/import.jpg)

「シンボル変数 wetravel_engage_home が見つかりません」と「シンボル変数 wetravel_engage_search が見つかりません」というエラーも表示されます。 これらを `Constant.java` ファイルに追加します（アプリ/src/メイン/java/com/wetravel/Utils 内）。

```java
public static final String wetravel_engage_home = "wetravel_engage_home";
public static final String wetravel_engage_search = "wetravel_engage_search";
```

![Constant.java ファイルに場所の名前を追加する ](assets/constants.jpg)

### バッチ・プリフェッチ・リクエスト・コードの説明

| コード | 説明 |
|--- |--- |
| `targetPrefetchContent()` | [!DNL Target] メソッドを使用して 2 つの [!DNL Target] しい場所を取得しキャッシュするユーザー定義関数（SDKの一部ではない）。 |
| `prefetchContent()` | プリフェッチリクエストを送信する [!DNL Target] SDK メソッド |
| `Constant.wetravel_engage_home` | ホーム画面 [!DNL Target] オファーコンテンツを表示する、プリフェッチされた場所名 |
| `Constant.wetravel_engage_search` | 検索結果画面 [!DNL Target] オファーコンテンツを表示する、プリフェッチされた場所名。 これはプリフェッチの 2 番目の場所であるため、このプリフェッチ・リクエストは「プリフェッチ・バッチ・リクエスト」と呼ばれます。 |
| setUp （） | [!DNL Target] しいオファーがプリフェッチされた後にアプリのホーム画面をレンダリングするユーザー定義関数 |

### 非同期と同期の比較

先ほど実装したコードを使用して、ホーム画面がレンダリングされる直前に、プリフェッチリクエストは同期されたブロック呼び出しとして行われます。 新しいコードを HomeActivity コントローラーに貼り付けた際、`setUp()` 関数の実行を `onResume()` 関数から Target リクエストの後まで移動しました。 これは、アプリを初めて開いたときにコンテンツをパーソナライズするシナリオで役立ちます。この場合、最初の画面がレンダリングされる前に、Target サーバーからパーソナライズされたコンテンツが返された（またはタイムアウトした）ことを確認できるからです。 リクエストを（バックグラウンドで）非同期に読み込めるようにするには、代わりに `setUp()` 関数内で `onCreate()` を呼び出します。

### バッチ・プリフェッチ・リクエストの検証

アプリを再構築し、Android エミュレーターを開きます。 （以下のスクリーンショットでは、Android Q version 9+、API レベル 29 の Pixel 2 を使用しています）。 プリフェッチ応答は、「prefetch response received」と読みます。

ホーム画面がレンダリングされると、プリフェッチリクエストが読み込まれます。 Logcat で、[!DNL "Target"] をフィルタリングして、リクエストと応答を確認します。

![ ホーム画面でリクエストを検証 ](assets/prefetch_validation.jpg)

正常な応答が表示されない場合は、`ADBMobileConfig.json` ファイルの設定と HomeActivity ファイルのコード構文を確認してください。

2 つの場所がデバイスにキャッシュされるようになりました。 場所の名前は間もなく [!DNL Target] インターフェイスに遅延読み込みされ、アクティビティで使用する際に、様々なドロップダウンメニューで選択できるようになります。

### キャッシュされた各場所への読み込み要求の追加

ロケーションがプリフェッチされ、その応答がデバイスにキャッシュされたら、キャッシュからオファーコンテンツを取得する `Target.loadRequest()` メソッドを追加して、アプリケーションの更新に使用できます。 プリフェッチリクエストで実行される `engageMessage()` という新しいカスタムメソッドを追加します。 `engageMessage()` が `Target.loadRequest()` を呼び出します。 `engageMessage()` は `setUp()` の前に実行され、画面が設定される前に読み込みリクエストが呼び出されるようにします。

まず、HomeActivity で wetravel_engage_home ロケーションの `engageMessage()` 呼び出しとメソッドを追加します。

![ 最初の読み込みリクエストを追加 ](assets/wetravel_engage_home_loadRequest.jpg)

更新されたコードを次に示します。

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
    public void engageMessage() {
        Target.loadRequest(Constant.wetravel_engage_home, "", null, null, null,
            new Target.TargetCallback<String>(){
                @Override
                public void call(final String s) {
                    runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            System.out.println("Engage Message : " + s);
                            if(s != null && !s.isEmpty()) Utility.showToast(getApplicationContext(), s);
                        }
                    });
                }
            });
    }
```

次に、SearchBusActivity の wetravel_engage_search の場所に対する `engageMessage()` 呼び出しおよびメソッドを追加します。 `engageMessage()` の呼び出しは、`onResume()` の呼び出しの前に `setUpSearch()` メソッドで設定されるため、画面が設定される前に実行されます。

![2 回目の読み込みリクエストを追加 ](assets/wetravel_engage_search_loadRequest.jpg)

更新されたコードを次に示します。

```java
    @Override
    public void onResume() {
        super.onResume();
        engageMessage();
        setUpSearch();
    }
    public void engageMessage() {
        Target.loadRequest(Constant.wetravel_engage_search, "", null, null, null,
                new Target.TargetCallback<String>(){
                    @Override
                    public void call(final String s) {
                        runOnUiThread(new Runnable() {
                            @Override
                            public void run() {
                                System.out.println("Engage Message : " + s);
                                if(s != null && !s.isEmpty()) Utility.showToast(getApplicationContext(), s);
                            }
                        });
                    }
                });
    }
```

SearchBusActivity に Target メソッドを追加したので、必ず [!DNL Target] のクラスをインポートしてください。

```java
import com.adobe.mobile.Target;
import com.adobe.mobile.TargetPrefetchObject;
```

## リアルタイムリクエストの追加

アプリに追加する次のリクエストは、ありがとう画面のリアルタイムリクエストになります。 「リアルタイム」とは、リクエストが行われ、応答が直ちに適用される（後でキャッシュされない）ことを意味します。 後のレッスンでは、このリクエストを使用して、ユーザーの旅行先に合わせてパーソナライズされたエクスペリエンスを作成します。

次に、「ありがとうございます」画面でリアルタイムリクエストを追加します。 ThankYouActivity ファイルで、赤で表示されている変更を行います。
![ ありがとう画面でのリアルタイムの場所の追加 ](assets/thankyou.jpg)

ThankYouActivity ファイルの末尾までスクロールします。 `getRecommandations()` 関数の 3 行をコメントアウトし、`targetLoadRequest()` 関数の呼び出しを追加します。

```java
// AppDialogs.dialogLoaderHide();
// recommandations.addAll(recommandation.recommandations);
// recommandationbAdapter.notifyDataSetChanged();
```

次のコード行を `getRecommandations()` 関数に追加します。

```java
targetLoadRequest(recommandation.recommandations);
```

次に、`targetLoadRequest()` の関数を定義する必要があります。
![ ありがとう画面でのリアルタイムの場所の追加 ](assets/thankyou2.jpg)

`filterRecommendationBasedOnOffer()` 関数の後にこのコードブロックを追加します。

```java
public void targetLoadRequest(final ArrayList<Recommandation> recommandations) {
    Target.loadRequest(Constant.wetravel_context_dest, "", null, null, null, new Target.TargetCallback<String>() {
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
}
```

ThankYouActivity に Target メソッドを追加したので、必ず Target クラスを読み込んでください。

```java
import com.adobe.mobile.Target;
import com.adobe.mobile.TargetPrefetchObject;
```

### targetLoadRequest （） コードの説明

| コード | 説明 |
|--- |--- |
| `targetLoadRequest()` | wetravel_context_dest の場所を読み込んで表示する `Target.loadRequest()` に実行される（SDKの一部ではない）ユーザー定義関数 |
| `Target.loadRequest()` | Target サーバーにリクエストを送信するSDK メソッド |
| Constant.wetravel_context_dest | [!DNL Target] インターフェイスでアクティビティを作成する際に後で使用する、リクエストに割り当てられた場所の名前 |
| `filterRecommendationBasedOnOffer()` | Target の応答から場所のオファーを受け取り、オファーのコンテンツに基づいてアプリがどのように変化するかを決定する、アプリ内のユーザー定義関数 |
| `recommandations.addAll()` | ありがとう画面が読み込まれたときにデフォルトで実行されていましたが、`filterRecommendationBasedOnOffer()` が Target 応答を受信して解析した後で実行されるようになりましたアプリ内のユーザー定義関数 |

これは、アプリに加えた、ホーム画面に追加されたリクエストに対する、より高度なアップデートでした。ここで内容を確認してみましょう。

1. 以前、デフォルトの 3 つのプロモーションを表示していたアプリの動作を、コードの行をコメントアウトすることで中断しました
1. 代わりに、新しい関数を実行するようにアプリに指示し、targetLoadRequest を任意に命名しました
1. Target.loadRequest メソッドを使用して Target にリクエストを行い、`targetLoadRequest` のオファー応答を受信したら直ちに `filterRecommendationBasedOnOffer()` 関数を実行するように、[!DNL Target] 関数を定義しました
1. `filterRecommendationBasedOnOffer()` 関数は応答を解釈し、画面に適用するプロモーションを決定します。

これは、モバイルアプリで [!DNL Target] を使用する場合の非常に一般的な使用パターンです。  モバイルアプリのほぼすべての側面をパーソナライズできるという点で、どちらも非常に強力です。 また、アプリコードと、後で [!DNL Target] インターフェイスで定義するオファーとの調整も必要です。 この調整により、一部のパーソナライゼーションのユースケースでは、アクティビティを開始するために、アプリストアでアプリを更新する必要が生じる場合があります。

### リアルタイムリクエストの検証

Android エミュレーターを開き、次のすべての手順を実行して旅行を予約します。ホーム / バス検索結果/シートの選択、支払いオプション （空白のデータのあるすべての支払いオプションが機能します）。

最後の「ありがとうございます」画面で、Logcat の応答を確認します。 応答は、「Default content was returned for &quot;wetravel_context_dest&quot;:

![ ありがとう画面でのリアルタイムの場所の追加 ](assets/thankyou_validation.jpg)

## プリフェッチされた場所のキャッシュからの消去

セッション中に、プリフェッチされた場所をクリアする必要が生じる場合があります。 例えば、予約が発生した場合、ユーザーが「関与」し、予約プロセスを理解しているので、キャッシュされた場所をクリアすることは理にかなっています。 セッション中に別の旅行を予約する場合、予約のガイドとしてホーム画面と検索結果画面の元の場所は必要ありません。 キャッシュから場所をクリアし、おそらく割引された 2 回目の予約やその他の関連するシナリオのために、新しいオファーをプリフェッチする方が理にかなっています。 セッション中に予約が行われた場合に、新しい場所をプリフェッチするロジックをホーム画面と検索結果画面に追加できます。

この例では、予約が行われる際に、セッションのプリフェッチされた場所をクリアするだけです。 それには、`Target.clearPrefetchCache()` 関数を呼び出します。 次に示すように、`targetLoadRequest()` 関数内に関数を設定します。

```java
Target.clearPrefetchCache()
```

![ プリフェッチされた場所をキャッシュからクリア ](assets/clearPrefetch.jpg)

おめでとうございます。 アプリにパーソナライゼーションのフレームワークが追加されました。 次のレッスンでは、これらの場所にパラメーターを追加して、パーソナライゼーション機能を強化します。

**[次へ：「パラメーターの追加」 >](add-parameters.md)**
