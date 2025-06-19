# Promptfooによる評価


### 前提条件 
Promptfooを使用するには、システムにnode.jsとnpmがインストールされている必要があります。詳細については[このガイド](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)を参照してください。

npmを使用してpromptfooをインストールするか、npxを使用して直接実行できます。このガイドではnpxを使用します。

*注意: この例では`npx promptfoo@latest init`を実行する必要はありません。このディレクトリには既に初期化された`promptfooconfig.yaml`ファイルがあります*

公式ドキュメントは[こちら](https://www.promptfoo.dev/docs/getting-started)を参照してください


### 開始方法
評価は`promptfooconfig.yaml`ファイルによって制御されます。このファイルでは以下のセクションを定義します：

- プロンプト
    - Promptfooは様々な形式でプロンプトをインポートできます。詳細は[こちら](https://www.promptfoo.dev/docs/configuration/parameters)をお読みください。
    - この例では3つのプロンプトを読み込みます - `guide.ipynb`で使用されたものと同じで、`prompts.py`ファイルから：
        - これらの関数は`guide.ipynb`で使用されたものと同じですが、Anthropic APIを呼び出す代わりにプロンプトを返すだけです。PromptfooがAPIの呼び出しと結果の保存を管理します。
        - プロンプト関数の詳細は[こちら](https://www.promptfoo.dev/docs/configuration/parameters#prompt-functions)をお読みください。Pythonを使用することで、RAGに必要なVectorDBクラスを再利用できます。これは`vectordb.py`で定義されています。
- プロバイダー
    - Promptfooでは様々なプラットフォームの多数のLLMに接続できます。詳細は[こちら](https://www.promptfoo.dev/docs/providers)を参照してください。`guide.ipynb`では温度設定0.0のデフォルトでHaikuを使用しました。Promptfooを使用して様々な温度設定を実験し、我々のユースケースに最適な選択を特定します。
- テスト
    - `guide.ipynb`で使用されたのと同じデータを使用します。これは[Google Sheet](https://docs.google.com/spreadsheets/d/1UwbrWCWsTFGVshyOfY2ywtf5BEt7pUcJEGYZDkfkufU/edit#gid=0)で確認できます。
    - Promptfooには[こちら](https://www.promptfoo.dev/docs/configuration/expected-outputs/deterministic)で確認できる豊富な組み込みテストがあります。
    - この例では、評価条件が各行で変わるため`dataset.csv`でテストを定義し、すべてのテストケースで一貫した条件については`promptfooconfig.yaml`でテストを定義します。詳細は[こちら](https://www.promptfoo.dev/docs/configuration/parameters/#import-from-csv)をお読みください。
- 変換
    - `defaultTest`セクションで変換関数を定義します。これはLLM応答からテストしたい特定の出力を抽出するPython関数です。
- 出力
    - 出力ファイルのパスを定義します。Promptfooは多くの形式で結果を出力できます。[こちら](https://www.promptfoo.dev/docs/configuration/parameters/#output-file)を参照してください。または、PromptfooのWeb UIを使用することもできます。[こちら](https://www.promptfoo.dev/docs/usage/web-ui)を参照してください。


### 評価の実行

Promptfooを開始するには、ターミナルを開いてこのディレクトリ（`./evaluation`）に移動してください。

評価を実行する前に、以下の環境変数を定義する必要があります：

`export ANTHROPIC_API_KEY=YOUR_API_KEY`  
`export VOYAGE_API_KEY=YOUR_API_KEY`

`evaluation`ディレクトリから以下のコマンドを実行してください。

`npx promptfoo@latest eval`

リクエストの並行性を増加させたい場合（デフォルト = 4）、以下のコマンドを実行してください。

`npx promptfoo@latest eval -j 25`  

評価が完了すると、ターミナルはデータセットの各行の結果を出力します。

これで`guide.ipynb`に戻って結果を分析できます！

