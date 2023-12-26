### カウンター画面の挙動説明（4時間）

1. ボタンを押してカウント値が画面に反映されるまでの流れを箇条書きで説明してください
> - プラス・マイナスボタンのタップイベントでlocalCounterProviderのそれぞれのメソッドを呼ぶ
> - localCounterProviderがintのステートをSharedPreferencesに保存
> - localCounterProviderをウォッチし画面を更新
2. Flutterのどのパッケージを使ってカウント値を永続化しているのか説明してください
> - shared_preferences
3. 永続化で使われているデータソースが、Android/iOSで異なります。それぞれのネイティブ層で使われているデータソース名を教えてください
> - NSUserDefaults, SharedPreferences

### Github Users画面の挙動説明（8時間）

1. 以下の操作をした際に画面にデータが反映されるまでの流れを箇条書きで説明してください

    1. 初回表示時
    > - githubUsersControllerProviderのbuildメソッドで、githubUsersControllerProviderのfetchUsersメソッドを呼んでデータを取得
    
    2. Pull To Refresh操作（下へ引っ張って更新する操作）
    > - SmartRefresher初期化時のonRefresh引数のFunctionで、githubUsersControllerProviderをinvalidate
    
    3. Paging操作（リストを上にスクロールして更新する操作）
    > - SmartRefresherのコンストラクタのonLoading引数のFunctionで、githubUsersControllerProviderのonFetchMoreメソッドを実行
    > - 既存のデータ末尾に新たなデータを追加
> - 共通: githubUsersControllerProviderをウォッチし画面を更新
2. データの取得先は`https://api.github.com` ですが、このAPIを実際に呼んでいる箇所はどこか教えてください
> - [github_api_client.dart#L35](https://github.com/hukusuke1007/flutter_app_template/blob/e315652655ced2f27a108379bff27ad588f433c2/feature_first/lib/features/github_users/repositories/github_api_client.dart#L35)
3. 本テンプレートではBearer認証は利用していませんが、Bearer Tokenを設定できる箇所を確認し、Bearer認証の用途を説明してください
> - [auth_header_interceptor.dart](https://github.com/hukusuke1007/flutter_app_template/blob/main/feature_first/lib/core/repositories/dio/auth_header_interceptor.dart)
> - [bearer認証.pdf](https://drive.google.com/file/d/1jCeMWNJrrxnkwt6GiE-lxYRwAo1FxhVr/view?usp=sharing)

### サインアップ画面の挙動説明（4時間）

1. メールアドレスとパスワードが正しいかどうか確認している箇所を教えてください（バリデーション処理）
> - それぞれのテキストフィールド初期化時のvalidator引数
2. 正しいメールアドレスとパスワードが入力された後の挙動を箇条書きで説明してください
> - firebaseAuthRepositoryProviderがサインアップ処理を実行
> - authStateControllerをアップデート
> - ダイアログを表示
3. SignUpWithEmailAndPassword内のロジックでエラーが発生した場合の挙動について箇条書きで説明してください
> - AuthErrorCodeのケースによってAppExceptionをthrow
> - ダイアログを表示

### タイムライン画面の挙動説明（8時間）

1. firestoreのインデックスを設定していない場合はエラーが発生します。その解決策を実施して説明してください。
> - createdAtフィールドのコレクショングループスコープを有効にする
> <img width="800" alt="index" src="https://github.com/skw398/profile/assets/114917347/20867156-cc2a-48f8-9530-0a9ccce1917f">

2. 以下の操作をした際に画面にデータが反映されるまでの流れを箇条書きで説明してください
    1. 初回表示時
    > - fetchTimelineProviderのbuildメソッドで、クエリを設定したリポジトリーがデータを取得
    > - fetchTimelinePostCountProviderが投稿数を取得
    2. Pull To Refresh操作（下へ引っ張って更新する操作）
    > - fetchTimelineProvider, fetchTimelinePostCountProviderをinvalidate
    3. Paging操作（リストを上にスクロールして更新する操作）
    > - fetchTimelineProviderのonFetchMoreメソッドを呼んでデータを取得
    > - 取得したデータを既存データの末尾に追加
> - 共通: fetchTimelineProvider, fetchTimelinePostCountProviderをウォッチして、画面を更新
3. 作成したデータを追加、更新、削除した後の挙動について箇条書きで説明してください。但し、複数の画面を跨いでの状態反映である事を踏まえて詳しく説明してください。
> - データベースを更新
> - fetchTimelineProvider, fetchTimelinePostCountProviderをinvalidateすることで、これらを購読している全ての箇所で変更を共有

### 画面遷移の挙動説明（8時間）

1. go_routerの以下の画面遷移方法について実装例を交えて説明してください
    1. パラメータ未付与して画面遷移
    ```dart
    GoRoute(
      path: '/users',
      builder: (context, state) => const UserScreen(),
    ),
    ```
    ```dart
    context.go('/users')
    ```
    2. パラメータ付与して画面遷移
    ```dart
    GoRoute(
      path: '/users/:userId',
      builder: (context, state) => const UserScreen(),
    ),
    ```
    ```dart
    context.go('/users/123')
    ```
2. 画面遷移にはモーダル遷移とプッシュ遷移があります、実施する方法はどうすれば良いか教えてください
> - プッシュ: `context.push()`　`context.go()`にパスを渡す(Navigation 1,2の違いに注意)
> - モーダル: MaterialPageを使用し、fullscreenDialogをtrueにする
3. 画面遷移のアニメーションを変更したい場合はどうすれば良いか説明してください
> - 遷移先のページをCustomTransitionPageでラップして、transitionsBuilderを設定する

### データモデルの挙動説明（4時間）

1. [freezed](https://pub.dev/packages/freezed)の特徴について説明してください
> - モデルに必要な便利メソッドやプロパティを自動生成
2. [equatable](https://pub.dev/packages/equatable)の特徴について説明してください
> - モデルのインスタンスを`==`で比較できるようにする  
3. イミュータブルプログラミングのメリットを説明してください
> - 複数の変数が同じ参照を共有する場合の、意図しない書き換えをふせぐ 
> - DartはSwiftのstructのような値型のクラスがないので、より意識する必要がある
4. freezedとequatableを利用する事で得られる恩恵を説明してください
> - freezedで生成されるイミュータブルなクラスとcopyWithメソッドによって、イミュータブルプログラミングが可能
> - equatableにより、copyWithメソッドによってコピーされたオブジェクトを前の状態などと比較できる
> - テストの際の比較も便利

### themeの挙動説明（4時間）

1. themeはどのような役割なのか説明してください
> - アプリ全体で共有するカラーやフォントを定義する
2. themeでナビゲーションバーの背景を変更する場合はどのような実装になるか書いてください
> - ライトモード、ダークモードそれぞれ以下の2行を変更する
>> - [theme.dart#L31](https://github.com/hukusuke1007/flutter_app_template/blob/e315652655ced2f27a108379bff27ad588f433c2/feature_first/lib/core/res/theme.dart#L31)
>> - [theme.dart#L87](https://github.com/hukusuke1007/flutter_app_template/blob/e315652655ced2f27a108379bff27ad588f433c2/feature_first/lib/core/res/theme.dart#L87)
