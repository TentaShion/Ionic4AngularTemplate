# Ionic4AngularTemplate
クロスプラットフォーム開発フレームワーク[Ionic][ionic] を、自分好みにカスタマイズし、初期テンプレートとして使えるようにしていくリポジトリです。
ベースのフレームワークとして[Angular][angular] を採用しています。

このテンプレートでは、下記が設定されています。

* Angular ベースのマルチプロジェクト構成になっています
* Capacitor の利用を前提としています<br />
→ SplashScreen, StatusBar はCapacitor のものに変更しています<br />
→ 実際に使う場合は、追加設定が必要です
* Docker コンテナを立ち上げることで、コマンド入力できるお試し環境が使えます
    * Angular CLI はGlobal インストールされているので```ng``` コマンドがどこでも使えます
    * Capacitor CLI はGlobal インストールされているので```cap``` コマンドがどこでも使えます
    * ```capacitor open```, ```ionic serve```, ```ng serve``` など自動で画面を開くコマンドは使えません
    * Ionic CLI がGlobal インストールされているので```ionic``` コマンドがどこでも使えます





## 想定している作業環境
ブログにまとめた「[macOS に自分好みのモバイルアプリ開発環境を整えてみた][moku_tech20190708095349]」をベースにしています。詳細はそちらをご確認ください。

項目 | macOS | Windows | 備考
--- | --- | --- | ---
Android Studio | [Android Studio 3.5][androidstudio] | ? | ローカル環境でAndroid アプリを開発する場合
Browser | [Google Chrome][chrome] の最新版 | ? | Android 開発時にも使うと便利です
Browser | [Safari][safari] の最新版 | × | iOS 開発時に使うと便利です
Docker | [Docker Desktop Community][docker] 2.1.0.3 | ? | 後術するお試し環境を利用する場合
Editor | [Visual Studio Code][vscode] の最新版 | ? |  |
Node | 10.16.3 | ? | 後術するお試し環境と揃えています
OS | [macOS Mojave][macos] | ? |  |
VSCode 拡張機能 | [Angular Language Service][vscode_angular] | ? |  |
VSCode 拡張機能 | [Debugger for Chrome][vscode_chrome] | ? |  |
VSCode 拡張機能 | [Docker][vscode_docker] | ? | 後術するお試し環境を利用する場合にオススメ
Xcode | [Xcode 11][xcode] | × | ローカル環境でiOS アプリを開発する場合





## 使い方
下記２つの使い方が出来ます。
モバイルネイティブを開発する場合は、Android Studio やXcode を使うので、ローカル環境で作業するのがオススメです。

1. Docker コンテナを立ち上げて、その中で作業する(お試し環境)
    * 下記コマンドは、Docker コンテナ内で表示されたものは見ることができないため、制約があります
        * ```capacitor open```(コンテナ内にAndroid Studio やXcode が存在しないので使えないです)
        * ```ionic serve```(サービングされているのでローカルのブラウザから見ることができます)
        * ```ng e2e```(コンテナ内にChrome が存在しないので使えないです)
        * ```ng serve```(サービングされているのでローカルのブラウザから見ることができます)
2. ローカル環境で作業する
    * 環境を整備すれば全てのコマンドが使えます


### お試し環境
1. あらかじめDocker エンジンを起動しておきます
2. ローカルで```ionic start "(project name)" https://github.com/TentaShion/Ionic4AngularTemplate --type=angular``` を実行し、このテンプレートを入手します
3. エラーが出ても気にせず、手順２で作成したワークスペースをVisual Studio Code で開きます
4. コミット履歴が引き継がれている状態なので、ローカルGit リポジトリを削除し、再度作り直します
5. ターミナルを開き```sh run-playground.sh``` を実行します
6. 手順４が完了するとDocker コンテナ内でコマンド入力できるようになるので、実装作業をしていきます
    * 最初は```node_modules``` をインストールしたいので、コンテナ内で```yarn install --frozen-lockfile``` を実行してください(チーム全体で揃えるため)<br />
        → もし実行時にエラーが発生したら、プロジェクト管理者に連絡し、ライブラリ依存関係の確認を依頼してください<br />
        → linux 環境でfsevents のインストールは時間がかかるので、気長にお待ちください
    * serve する際は下記のコマンドを入力し、Docker コンテナ外から接続できるようにしてください
        * ```ionic serve --address=0.0.0.0```
        * ```ng serve --address=0.0.0.0```
    * capacitor を使う場合の手順は下記を参考にしてください
        1. Capacitor を導入したいプロジェクトがある場所に移動します
        2. ```ionic integrations enable capacitor``` を実行します
        3. Global インストールされているものと重複するので、手順１で移動した場所で下記を変更します
            * ```node_modules``` を削除します
            * ```package.json``` をリセットします
            * ```yarn.lock``` を削除します
        4. ```cap init``` を実行します
        5. ```ionic build``` を実行します(※パスは適宜通してください)
        6. ```ionic capacitor add (platform)``` を実行します(※パスは適宜通してください)
        7. これでプロジェクト自体は生成できますが、ここから先はローカル環境で実行してください
7. 作業が終わったら、Docker コンテナ内で```exit``` を入力します


### ローカル環境
1. ```Playground.Dockerfile``` の内容を確認し同じ環境を整備します
2. ローカルで```ionic start "(project name)" https://github.com/TentaShion/Ionic4AngularTemplate --type=angular``` を実行し、このテンプレートを入手します
3. エラーが出ても気にせず、手順２で作成したワークスペースをVisual Studio Code で開きます
4. コミット履歴が引き継がれている状態なので、ローカルGit リポジトリを削除し、再度作り直します
5. ```cd workspace``` で作業場所に移動します
6. 作業していきます





## 開発メモ
### プロジェクト構成
自分より若い世代のプロジェクトのみ参照できる前提で、下記のようなプロジェクト構成を取っています。
プロジェクトの詳細は「パス」欄のREADME へのリンクより確認してください。

世代 | パス | フレームワーク | 単体テスト設定 | e2e 設定 | 用途
:---: | --- | --- | :---: | :---: | ---
\- | [./workspace/](./workspace/README.md) | Angular | なし | なし | マルチプロジェクト構成のルート
1 | [./workspace/projects/core/](./workspace/projects/core/README.md) | Angular(Library) | なし | なし | アプリで使うデータと、アプリの使い方の定義
2 | [./workspace/projects/datasource/](./workspace/projects/datasource/README.md) | Angular(Library) | あり | なし | アプリのビジネスロジックの実装
3 | [./workspace/projects/main/](./workspace/projects/main/README.md) | Ionic | あり | あり | アプリ本体の実装


### Visual Studio Code
* 必要な拡張機能は```.vscode/extensions.json``` に記載したので、必要に応じて編集してください
* ワークスペースを識別しやすくするためにカラーテーマを変更しています。必要に応じて```.vscode/setting.json``` を編集してください





[androidstudio]: https://developer.android.com/studio?hl=ja
[angular]: https://angular.jp/
[chrome]: https://www.google.com/intl/ja_jp/chrome/
[docker]: https://www.docker.com/products/docker-desktop
[ionic]: https://ionicframework.com/
[macos]: https://www.apple.com/jp/macos/mojave/
[moku_tech20190708095349]: https://mokumokulog.netlify.com/tech/20190708095349
[safari]: https://www.apple.com/jp/safari/
[vscode]: https://code.visualstudio.com/
[vscode_angular]: https://marketplace.visualstudio.com/items?itemName=Angular.ng-template
[vscode_chrome]: https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome
[vscode_docker]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker
[xcode]: https://developer.apple.com/jp/xcode/
