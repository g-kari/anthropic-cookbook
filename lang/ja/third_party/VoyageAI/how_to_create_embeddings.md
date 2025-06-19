# 埋め込み
テキスト埋め込みは、浮動小数点数のベクトルとして表現される、テキスト文字列の数値表現です。2つのテキスト埋め込み間の距離（一般的にはコサイン類似度）を使用して、2つのテキストがどの程度関連しているかを測定できます。距離が小さいほど関連性が高いと予測されます。

文字列の類似性を比較したり、文字列間の距離でクラスタリングすることで、**検索**（RAGアーキテクチャで人気）、**推薦**、**異常検出**を含む幅広いアプリケーションが可能になります。

## Anthropicで埋め込みを取得する方法
Anthropicは独自の埋め込みモデルを提供していませんが、テキスト埋め込みの優先プロバイダーとして[Voyage AI](https://www.voyageai.com/?ref=anthropic)と提携しています。Voyageは[最先端](https://blog.voyageai.com/2023/10/29/voyage-embeddings/?ref=anthropic)の埋め込みモデルを作成しており、金融や医療などの特定業界ドメイン向けにカスタマイズされたモデルや、あなたの会社向けにファインチューニングできるモデルも提供しています。

Voyage埋め込みにアクセスするには、まず[Voyage AIのウェブサイト](https://dash.voyageai.com/?ref=anthropic)でサインアップし、APIキーを取得し、便宜上APIキーを環境変数として設定してください：

```bash
export VOYAGE_API_KEY="<your secret key>"
```

以下に説明するように、公式[`voyageai` Pythonパッケージ](https://github.com/voyage-ai/voyageai-python)またはHTTPリクエストを使用して埋め込みを取得できます。

### Voyage Pythonパッケージ

`voyageai`パッケージは以下のコマンドでインストールできます：

```bash
pip install -U voyageai
```

その後、クライアントオブジェクトを作成し、テキストの埋め込みに使用を開始できます：

```python
import voyageai

vo = voyageai.Client()
# これは自動的に環境変数VOYAGE_API_KEYを使用します。
# または、vo = voyageai.Client(api_key="<your secret key>")を使用できます

texts = ["Sample text 1", "Sample text 2"]

result = vo.embed(texts, model="voyage-2", input_type="document")
print(result.embeddings[0])
print(result.embeddings[1])
```

`result.embeddings`は、それぞれ1024の浮動小数点数を含む2つの埋め込みベクトルのリストになります。上記のコードを実行すると、2つの埋め込みが画面に出力されます：

```
[0.02012746, 0.01957859, ...]  # "Sample text 1"の埋め込み
[0.01429677, 0.03077182, ...]  # "Sample text 2"の埋め込み
```

埋め込みを作成する際、`embed()`関数にいくつかの他の引数を指定することができます。仕様は以下の通りです：

> `voyageai.Client.embed(texts : List[str], model : str = "voyage-2", input_type : Optional[str] = None, truncation : Optional[bool] = None)`

- **texts** (List[str]) - `["I like cats", "I also like dogs"]`のような文字列のリストとしてのテキストのリスト。現在、リストの最大長は128で、リスト内のトークンの総数は`voyage-2`で最大320K、`voyage-code-2`で120Kです。
- **model** (str) - モデル名。推奨オプション：`voyage-2`（デフォルト）、`voyage-code-2`。
- **input_type** (str, オプション, デフォルトは`None`) - 入力テキストのタイプ。デフォルトは`None`。その他のオプション：`query`、`document`。
    - input_typeが`None`に設定されている場合、入力テキストは埋め込みモデルによって直接エンコードされます。代替的に、入力が文書またはクエリの場合、ユーザーはinput_typeをそれぞれ`query`または`document`に指定できます。そのような場合、Voyageは入力テキストに特別なプロンプトを前置し、拡張された入力を埋め込みモデルに送信します。
    - 検索/検索ユースケースでは、検索品質を向上させるためにクエリまたは文書をエンコードする際にこの引数を指定することを推奨します。input_type引数を使用して生成された埋め込みと使用せずに生成された埋め込みは互換性があります。

- **truncation** (bool, オプション, デフォルトは`None`) - 入力テキストをコンテキスト長に収まるように切り詰めるかどうか。
    - `True`の場合、長すぎる入力テキストは埋め込みモデルによってベクトル化される前にコンテキスト長に収まるように切り詰められます。
    - `False`の場合、与えられたテキストがコンテキスト長を超える場合にエラーが発生します。
    - 指定されていない場合（デフォルトは`None`）、Voyageはコンテキストウィンドウの長さをわずかに超える場合は埋め込みモデルに送信する前に入力テキストを切り詰めます。大幅に超える場合は、エラーが発生します。

### Voyage HTTP API

Voyage HTTP APIをリクエストして埋め込みを取得することもできます。例えば、ターミナルで`curl`コマンドを通じてHTTPリクエストを送信できます：

```bash
curl https://api.voyageai.com/v1/embeddings \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $VOYAGE_API_KEY" \
  -d '{
    "input": ["Sample text 1", "Sample text 2"],
    "model": "voyage-2"
  }'
```

得られる応答は、埋め込みとトークン使用量を含むJSONオブジェクトです：

```bash
{
  "object": "list",
  "data": [
    {
      "embedding": [0.02012746, 0.01957859, ...],
      "index": 0
    },
    {
      "embedding": [0.01429677, 0.03077182, ...],
      "index": 1
    }
  ],
  "model": "voyage-2",
  "usage": {
    "total_tokens": 10
  }
}
```

Voyage AIの埋め込みエンドポイントは`https://api.voyageai.com/v1/embeddings`（POST）です。リクエストヘッダーにはAPIキーが含まれている必要があります。リクエストボディは以下の引数を含むJSONオブジェクトです：

- **input** (str, List[str]) - 単一のテキスト文字列、または文字列のリストとしてのテキストのリスト。現在、リストの最大長は128で、リスト内のトークンの総数は`voyage-2`で最大320K、`voyage-code-2`で120Kです。
- **model** (str) - モデル名。推奨オプション：`voyage-2`（デフォルト）、`voyage-code-2`。
- **input_type** (str, オプション, デフォルトは`None`) - 入力テキストのタイプ。デフォルトは`None`。その他のオプション：`query`、`document`。
- **truncation** (bool, オプション, デフォルトは`None`) - 入力テキストをコンテキスト長に収まるように切り詰めるかどうか。
    - `True`の場合、長すぎる入力テキストは埋め込みモデルによってベクトル化される前にコンテキスト長に収まるように切り詰められます。
    - `False`の場合、与えられたテキストがコンテキスト長を超える場合にエラーが発生します。
    - 指定されていない場合（デフォルトは`None`）、Voyageはコンテキストウィンドウの長さをわずかに超える場合は埋め込みモデルに送信する前に入力テキストを切り詰めます。大幅に超える場合は、エラーが発生します。
- **encoding_format** (str, オプション, デフォルトは`None`) - 埋め込みがエンコードされる形式。Voyageは現在2つのオプションをサポートしています：
    - 指定されていない場合（デフォルトは`None`）：埋め込みは浮動小数点数のリストとして表現されます；
    - `"base64"`：埋め込みは[Base64](https://docs.python.org/3/library/base64.html)エンコーディングに圧縮されます。


### AWSマーケットプレイス

Voyage埋め込みは[AWSマーケットプレイス](https://aws.amazon.com/marketplace/seller-profile?id=seller-snt4gb6fd7ljg)で利用可能です。AWS上でVoyageにアクセスするための指示は以下の通りです：

1. モデルパッケージにサブスクライブ

    1. [モデルパッケージリストページ](https://aws.amazon.com/marketplace/seller-profile?id=seller-snt4gb6fd7ljg)に移動し、デプロイするモデルを選択します。
    1. *Continue to subscribe*ボタンをクリックします。
    1. *Subscribe to this software*ページで、詳細を慎重に確認してください。あなたとあなたの組織が標準エンドユーザーライセンス契約（EULA）、価格設定、サポート条件に同意する場合、「Accept Offer」をクリックしてください。
    1. *Continue to configuration*を選択してリージョンを選択すると、Product Arnが表示されます。これはBoto3を使用してデプロイ可能なモデルを作成するために必要なモデルパッケージARNです。選択したリージョンに対応するARNをコピーし、後続のセルで使用してください。

2. モデルパッケージをデプロイ

    ここからは、[Sagemaker Studio](https://aws.amazon.com/sagemaker/studio/)で提供されている[ノートブック](https://github.com/voyage-ai/voyageai-aws/blob/main/notebooks/deploy_voyage_code_2_sagemaker.ipynb)を続けることをお勧めします。JupyterLabスペースを作成し、ノートブックをアップロードして、そこから続けてください。


## 利用可能なモデル

Voyageは以下の埋め込みモデルの使用を推奨しています：

|  モデル | コンテキスト長 | 埋め込み次元 | 説明 |
| --- | --- | --- | --- |
| `voyage-2` | 4000 | 1024 | 最高の検索品質を持つ最新のベース（汎用）埋め込みモデル。詳細は[ブログ投稿](https://blog.voyageai.com/2023/10/29/voyage-embeddings/?ref=anthropic)を参照してください。 |
| `voyage-code-2` | 16000 | 1536 | コード検索に最適化（代替品より17%優れている）され、汎用コーパスでも最先端。詳細は[ブログ投稿](https://blog.voyageai.com/2024/01/23/voyage-code-2-elevate-your-code-retrieval/?ref=anthropic)を参照してください。 |

`voyage-2`は汎用埋め込みモデルで、ドメイン間で最先端のパフォーマンスを達成し、高効率を維持します。`voyage-code-2`はコードアプリケーションに最適化されており、より柔軟な使用のために4倍のコンテキスト長を提供しますが、わずかに高いレイテンシがあります。

Voyageはより高度で専門化されたモデルを積極的に開発しており、あなたの会社のために埋め込みをファインチューニングできます。トライアルアクセスまたは独自データでのファインチューニングについては、[contact@voyageai.com](mailto:contact@voyageai.com)にメールしてください！

- `voyage-finance-2`: 近日公開
- `voyage-law-2`: 近日公開
- `voyage-multilingual-2`: 近日公開
- `voyage-healthcare-2`: 近日公開

## 動機付けの例
埋め込みの取得方法がわかったので、簡単な動機付けの例を見てみましょう。

検索対象として6つの文書の小さなコーパスがあるとします

```python
documents = [
    "The Mediterranean diet emphasizes fish, olive oil, and vegetables, believed to reduce chronic diseases.",
    "Photosynthesis in plants converts light energy into glucose and produces essential oxygen.",
    "20th-century innovations, from radios to smartphones, centered on electronic advancements.",
    "Rivers provide water, irrigation, and habitat for aquatic species, vital for ecosystems.",
    "Apple's conference call to discuss fourth fiscal quarter results and business updates is scheduled for Thursday, November 2, 2023 at 2:00 p.m. PT / 5:00 p.m. ET.",
    "Shakespeare's works, like 'Hamlet' and 'A Midsummer Night's Dream,' endure in literature."
]
```

まずVoyageを使用してそれぞれを埋め込みベクトルに変換します

```python
import voyageai

vo = voyageai.Client()

# 文書を埋め込み
doc_embds = vo.embed(
    documents, model="voyage-2", input_type="document"
).embeddings
```

埋め込みにより、ベクトル空間でセマンティック検索/検索を行うことができます。例のクエリを考えると、

```python
query = "When is Apple's conference call scheduled?"
```

それを埋め込みに変換し、埋め込み空間での距離に基づいて最も関連性の高い文書を見つけるために最近傍検索を実行します。

```python
import numpy as np

# クエリを埋め込み
query_embd = vo.embed(
    [query], model="voyage-2", input_type="query"
).embeddings[0]

# 類似性を計算
# Voyage埋め込みは長さ1に正規化されているため、内積と
# コサイン類似度は同じです。
similarities = np.dot(doc_embds, query_embd)

retrieved_id = np.argmax(similarities)
print(documents[retrieved_id])
```

文書とクエリの埋め込みにそれぞれ`input_type="document"`と`input_type="query"`を使用することに注意してください。詳細な仕様は[こちら](#voyage-python-package)で確認できます。

出力は5番目の文書で、実際にクエリに最も関連性があります：

```
Apple's conference call to discuss fourth fiscal quarter results and business updates is scheduled for Thursday, November 2, 2023 at 2:00 p.m. PT / 5:00 p.m. ET.
```

ベクトルデータベースを含む埋め込みでRAGを行う方法の詳細なクックブックセットをお探しの場合は、[RAGクックブック](https://github.com/anthropics/anthropic-cookbook/blob/main/third_party/Pinecone/rag_using_pinecone.ipynb)をご確認ください。

## よくある質問
### 2つの埋め込みベクトル間の距離を計算するにはどうすればよいですか？
コサイン類似度が人気の選択ですが、ほとんどの距離関数で問題ありません。Voyage埋め込みは長さ1に正規化されているため、コサイン類似度は本質的に2つのベクトル間の内積と同じです。2つの埋め込みベクトル間のコサイン類似度を計算するために使用できるコードスニペットは以下の通りです。

```python
import numpy

similarity = np.dot(embd1, embd2)
# Voyage埋め込みは長さ1に正規化されているため、コサイン類似度は
# 内積と同じです。
```

大きなコーパス上でK個の最近傍埋め込みベクトルを見つけたい場合は、ほとんどのベクトルデータベースに組み込まれた機能を使用することをお勧めします。

### 埋め込み前に文字列のトークン数をカウントできますか？
はい！以下のコードで実行できます。

```python
import voyageai

vo = voyageai.Client()
total_tokens = vo.count_tokens(["Sample text"])
```

## 価格設定
価格情報はVoyageウェブサイトの[価格ページ](https://docs.voyageai.com/pricing/?ref=anthropic)で利用可能で、そこで確認する必要があります。