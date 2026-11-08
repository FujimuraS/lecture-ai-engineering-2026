# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：
- `--trace` で見えたツール呼び出し（ツール名と引数）：
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
[tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラ…
[tool return] search_glossary -> [{"term": "QLoRA", "lecture": 2, "definition": "ベースモデルの重みを4ビットなどに量子化してメモリを節約したうえで、LoRAで学習する手法。より大きなモデルを小さなGPUで扱える。本講座の第2…
[usage] RunUsage(cost=Decimal('0.0007688'), details={'text_prompt_tokens': 1196}, input_text_tokens=1196, input_tokens=1196, output_tokens=164, requests=2, tool_calls=2)
- 1つの質問での LLM 呼び出し回数（`requests`）：2回
- 追加したツールと、docstring に書いた説明：
[make_quiz]
  指定した回の用語から1つを選び、
  確認問題と答えを返す。
  「第2回の確認問題を出して」のように、
  復習用の問題を求められたときに使う。
  引数: {"lecture": {"type": "integer"}}


## 宿題

- 取り組んだレベル：基礎<br>
- やったこと：<br>
質問："LoRAとQLoRAの違いは？"<br>
以下の通り書き換え<br>
agent.pyのINSTRUCTIONSに、以下の文を加えた。<br>
- 用語集の内容をそのまま答えるのではなく、与えられた質問に忠実に答える。例えば「との違いは
」と聞かれたら、両方の言葉を用語集から引っ張ってきて並べるのではなく、違いだけ簡潔に示す。<br>

- 結果：<br>
書き換え前<br>
LoRAとQLoRAの主な違いは、**ベースモデルの重みの量子化（ビット数の削減）を行うかどうか**にあります。

* **LoRA**:
  事前学習済みのベースモデルの重みはそのまま固定（通常は16ビットなどの精度）にし、重みの更新分（低ランク行列）だけを新しく追加して学習するPEFT（Parameter-Efficient Fine-Tuning）手法です。学習パラメータやメモリを大幅に削減できます。
* **QLoRA**:
  LoRAの仕組みをベースにしつつ、**ベースモデルの重みをさらに4ビットなどに量子化**してメモリ消費を極限まで抑えたうえで、LoRAによるファインチューニングを行う手法です。これにより、より大きなモデルを比較的小さなGPUメモリでも扱えるようになります。

関係する回：第2回

--- trace ---
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
[tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラ…
[tool return] search_glossary -> [{"term": "QLoRA", "lecture": 2, "definition": "ベースモデルの重みを4ビットなどに量子化してメモリを節約したうえで、LoRAで学習する手法。より大きなモデルを小さなGPUで扱える。本講座の第2…
[usage] RunUsage(cost=Decimal('0.0009525'), details={'text_prompt_tokens': 1350}, input_text_tokens=1350, input_tokens=1350, output_tokens=219, requests=2, tool_calls=2)<br>
書き換え後<br>
LoRAとQLoRAの違いは、**ベースモデルの重みを量子化しているかどうか**です。

* **LoRA**：事前学習済みの重みを固定したまま、更新分（低ランク行列）のみを学習します。
* **QLoRA**：ベースモデルの重みを4ビットなどに量子化してメモリを大幅に節約した上で、LoRAと同様に学習を行います。これにより、より大きなモデルを小さなGPUで扱えるようになります。

（第2回）

--- trace ---
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
[tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行列だけを学習するPEFT手法。学習するパラ…
[tool return] search_glossary -> [{"term": "QLoRA", "lecture": 2, "definition": "ベースモデルの重みを4ビットなどに量子化してメモリを節約したうえで、LoRAで学習する手法。より大きなモデルを小さなGPUで扱える。本講座の第2…
[usage] RunUsage(cost=Decimal('0.0007955'), details={'text_prompt_tokens': 1460}, input_text_tokens=1460, input_tokens=1460, output_tokens=143, requests=2, tool_calls=2)
- 気づき・疑問：<br>
書き換え後は以下のように非常にわかりやすく簡潔に答えを与えてくれるようになった。他方で書き換え前の方が、より正確な表現になっていたように思う。これは「違いだけを簡潔に示す」と言う指示を出したためではないかと考える。toolの呼ばれ方は変わらなかった<br>
書き換え前：LoRAとQLoRAの主な違いは、**ベースモデルの重みの量子化（ビット数の削減）を行うかどうか**にあります。<br>
書き換え後：LoRAとQLoRAの違いは、**ベースモデルの重みを量子化しているかどうか**です。
<br>

