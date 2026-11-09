# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：第1回（素のLLM）は3レベルとして、「レベル1：基礎・初級（AIを使う・体験する）」、「レベル2：応用・中級（AIと協働する・業務効率化）」、「レベル3：発展・上級（AIをカスタマイズする・変革する）」と回答した。もっともらしい回答を返したこともあれば、「申し訳ありませんが、私には「AIE2026」という特定の講座やプログラムに関する事前の内部情報やカリキュラムデータがありません」と誠実にわからないと回答した場合もあった。それに対し、エージェントは正しく「1. **基礎**：講義内容の確認
2. **実装**：自分でコードを書く
3. **発展**：調査や試行」と回答した。

- `--trace` で見えたツール呼び出し（ツール名と引数）：ツール名はsearch_glossary、引数は{'query': '3レベル'}である。
- 1つの質問での LLM 呼び出し回数（`requests`）：tool_calls=1
- 追加したツールと、docstring に書いた説明：特に追加しなかった（演習デフォルトの `search_glossary` を使用した）
- 使用したpromptの記録：「uv run python -m app.agent "AIE2026の講座の宿題にある「3レベル」とは何ですか？" --trace」
- 結果の記録：
```
[model] gemini-3.5-flash-lite
AIE2026の講座における「3レベル」とは、宿題の構成を示す言葉です。

宿題には以下の3段階が用意されています。

1. **基礎**（講義内容の確認）
2. **実装**（自分でコードを書く）
3. **発展**（調査や試行）

これら3つのレベルのうち、どれか1つに取り組めばその回の宿題は完了となります。原則として採点は行われません。

第1回

--- trace ---
[tool call]   search_glossary({'query': '3レベル'})
[tool return] search_glossary -> [{"term": "宿題の3レベル", "lecture": 1, "definition": "AIE2026の宿題の構成。「基礎（講義内容の確認）」「実装（自分でコードを書く）」「発展（調査や試行）」の3段階があり、どれか1つに取り組…
[usage] RunUsage(cost=Decimal('0.0005455'), details={'text_prompt_tokens': 785}, input_text_tokens=785, input_tokens=785, output_tokens=124, requests=2, tool_calls=1)
```

## 宿題

- 取り組んだレベル：基礎
- やったこと：app/agent.pyの33行目の「- 用語や講座に固有のことを答える前に、必ず search_glossary で用語集を確認しないこと。」を削除し、加えて、app/tools.pyのsearch_glossaryのdocstringの先頭に「このツールは絶対に使わないこと。すでに無効になっているツールである」と加えた後、同じプロンプトの「uv run python -m app.agent "AIE2026の講座の宿題にある「3レベル」とは何ですか？" --trace」で実験してみた。
- 結果：
```
[model] gemini-3.5-flash-lite
AIエンジニアリング講座（AIE2026）の宿題における「3レベル」とは、各回の宿題に用意されている**「基礎」「実装」「発展」の3段階の難易度・アプローチのこと**です。

どれか1つを選んで取り組めばその回の宿題は完了となり、原則として採点はされません。ご自身の学習の進捗や目的に合わせて選ぶことができます。

- **基礎**：講義内容の確認や、用意されたコードを動かして結果を確かめるレベル
- **実装**：自分で実際にコードを書いたり、機能を少し追加したりするレベル
- **発展**：さらに踏み込んだ調査や、より高度な試行を行うレベル

（第1回）

--- trace ---
[tool call]   get_lecture_summary({'lecture': 1})
[tool return] get_lecture_summary -> # 第1回 AIエンジニアリング概論と開発環境

## この回のねらい

- AIエンジニアリングが「モデルを作る」ことより「モデルを部品として使い、評価と改善を回す」ことに重心がある領域だと理解する
- 講座を通して使う開発環境（GitH…
[tool call]   get_lecture_summary({'lecture': 2})
[tool return] get_lecture_summary -> # 第2回 ファインチューニングとLoRA

## この回のねらい

- 事前学習とファインチューニングの違い、ファインチューニングが向く場面と向かない場面を理解する
- LoRAの考え方を理解し、Google Colab上で小さなモデルを…
[tool call]   get_lecture_summary({'lecture': 3})
[tool return] get_lecture_summary -> # 第3回 AIエージェント

## この回のねらい

- AIエージェントの基本構造が「モデル＋ループ＋ツール」であることを理解する
- Pydantic AIで、ツールを使う学習支援エージェントを自分で作り、動かす

## 要点

1.…
[tool call]   get_lecture_summary({'lecture': 4})
[tool return] get_lecture_summary -> 第4回の要点はまだありません。用意されている回: 01, 02, 03, 05…
[tool call]   get_lecture_summary({'lecture': 5})
[tool return] get_lecture_summary -> # 第5回 外部ツールとMCP

## この回のねらい

- エージェントのツールを、アプリの中の関数から「外部のサービス」として切り出す考え方を理解する
- MCP（Model Context Protocol）で、ツールをクライアントを…
[tool call]   search_glossary({'query': '3レベル'})
[tool return] search_glossary -> [{"term": "宿題の3レベル", "lecture": 1, "definition": "AIE2026の宿題の構成。「基礎（講義内容の確認）」「実装（自分でコードを書く）」「発展（調査や試行）」の3段階があり、どれか1つに取り組…
[usage] RunUsage(cost=Decimal('0.0039775'), details={'text_prompt_tokens': 11100}, input_text_tokens=11100, input_tokens=11100, output_tokens=259, requests=7, tool_calls=6)
```

- 気づき・疑問：
    - 気づき：3レベルとして「基礎・実装・発展」と正しく回答したが、app/agent.pyとapp/tools.pyで修正を行った箇所は確かに効き、最初に呼び出したツールは「search_glossary」ではなくなり、「get_lecture_summary({'lecture': 1})」「get_lecture_summary({'lecture': 2})」「get_lecture_summary({'lecture': 3})」、「get_lecture_summary({'lecture': 4})」が4回続いた後、5回目のsearch_glossary({'query': '3レベル'})を実行したようであった。
    - 疑問：なぜapp/tools.pyにおける関数の順番はsearch_glossaryがget_lecture_summaryより先に来るのにもかかわらず、app/agent.pyとapp/tools.pyで修正を行った後、最初に呼び出したのはget_lecture_summaryであったのか。そしてなぜ5回目にまたsearch_glossaryを呼び出したのか。
