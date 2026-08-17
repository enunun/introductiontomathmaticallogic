# 6.2節（計算可能関数）作業ログ

対象ファイル: `src/contents/computable.tex`（本文), `src/contents/answer.tex`（演習解答), `src/main.tex`（マクロ)

## 背景

`computable.tex` の第6.2節「計算可能関数」は，導出可能性（形式的体系 \(\symcal{R}\) における \(E \evaluatable e\)）を用いて計算可能関数を定義し，帰納的関数がすべて計算可能関数であることを示す節である．
このログは，この節を書き進める中で行った作業の経緯と，現時点で未着手の課題をまとめたものである．

## これまでの作業（時系列）

1. **6.2節本体の新規執筆**
   `Def:computablefunction`（計算可能関数の定義）以降を新規に書き，帰納的関数がすべて計算可能関数であることを `Def:recursivefunction` の規則1〜6に関する構成的帰納法で証明した．

2. **証明の書き直し（論法・体裁）**
   「〜以外ありえない」という排他的な言い回しを多用していた箇所を，導出の形に関する補題（後の `lemma:evaluationderivationshape`）を用いた直接的な議論に書き換えた．
   また `enumerate` による箇条書きで書かれていた6つの場合分けを，読みやすさと行幅確保のため段落形式に変更した．

3. **未定義語の解消**
   「置換」「主要な関数記号」という，定義されないまま使われていた語を解消．
   「置換」は (SUB) の適用回数による言い回しに置き換え，「主要関数記号」は `Def:computablefunctionheadfunctionsymbol` として正式に定義した．

4. **定理の分割**
   「帰納的関数はすべて計算可能関数である」の証明が長いため，
   - `Thm:primitiverecursivefunctioncomputable`（原始帰納的関数の場合，規則1〜5）
   - `Thm:recursivefunctioncomputable`（一般の帰納的関数の場合，規則6を追加）

   の2段階に分割した．さらに規則1〜3（ゼロ関数・後者関数・射影関数）は機械的な内容のため `Que:basicfunctionscomputable` という演習に切り出した．

5. **演習解答の追加**
   `answer.tex` の `\Cref{chap:computable}` 節（それまで空だった）に，上記演習の解答を追加．追加漏れ（演習6.2.2の解答）も後で補った．

6. **追加演習2問の新設**
   - `Que:additioncomputable`：例6.2.9/6.2.10（\(f(x,0)=x,\ f(x,y+1)=f(x,y)+1\) の例）が実際に加法の計算可能性の例になっていることを証明させる演習．
   - `Que:fibonaccicomputable`：フィボナッチ数列の計算可能性を証明させる演習（2つの相互参照する関数記号で \((\Fib(n), \Fib(n+1))\) の組を計算する，というヒント付き）．

   `\Fib` マクロを `main.tex` に追加し，それぞれの解答を `answer.tex` に追加した．

7. **REPL規則の見落としに起因するバグの監査・修正**
   (REPL) は第2前提 `apply{f}{n_1,...,n_r} evaluate n` の引数 \(n_1,\dots,n_r\) が**すべて数項**でなければ適用できない，という制約に反する箇所を洗い出して修正した．
   - `Def:computablefunctionreplacement`：余分な閉じ括弧（LaTeXの構文ミス）と，「\(f\)を変数記号」という誤記（正しくは関数記号），および `Def:computablefunctionsubstitution` からのコピペ残骸（「\(t\)に現れるすべての\(a\)を」）を修正．
   - 例6.2.9 (`Ex:computablefunctionevaluatable`)：変数 \(a_0\) を残したまま (REPL) を適用していた導出図を，\(a_0\) にも先に (SUB) を適用してから (REPL) するよう修正．固定された自然数 \(x\) を導入する形に変更．
   - 例6.2.10 (`Ex:computablefunctionevaluatable2`)：同様の誤りが3箇所あったため，\(a_0 \mapsto \numeral{2}\) を毎回のSUBに含める形に修正（ステップ数は9のまま維持）．
   - 上記の修正に伴い，`answer.tex` 側の演習6.2.1（当時）の解答の説明文も整合するよう修正．
   - 補題6.2.12/6.2.14，定理6.2.15/6.2.16，演習6.2.2〜6.2.4とその解答は監査の結果すでに正しく書けていたため変更なし．

8. **演習の削除**
   手順7の修正により，旧 `Que:evaluationderivationshapeexample`（例6.2.10の導出をSUB→REPLの順に書き直させる演習）の前提が実質的に崩れた．
   修正後の例6.2.10はすでに \(f(2,3)=5\) の導出が「1回の(SUB) → 1回の(REPL)」という形（`lemma:evaluationderivationshape` そのもの）になっており，この演習が求める作業とほぼ同じ内容を本文が直接示してしまっていたため，演習・解答ともに削除した．

## 現在の6.2節の構成

| 番号 | ラベル | 内容 |
|---|---|---|
| 定義6.2.1 | `Def:computablefunctionterm` | \(\symcal{R}\) における項 |
| 定義6.2.2 | `Def:computablefunctionnumeral` | 数項 |
| 定義6.2.3 | `Def:evaluation` | 評価式 |
| 定義6.2.4 | `Def:computablefunctionsubstitution` | (SUB) で使う置き換え `Sub` |
| 定義6.2.5 | `Def:computablefunctionreplacement` | (REPL) で使う置き換え `Rep`（今回記述修正） |
| 定義6.2.6 | `Def:computablefunctioninference` | 推論規則 (SUB), (REPL) |
| 定義6.2.7 | `Def:evaluatable` | 導出図 |
| 定義6.2.8 | `Def:computablefunctionevaluatable` | 導出可能性 \(E \evaluatable e\) |
| 例6.2.9 | `Ex:computablefunctionevaluatable` | 導出図の例（今回導出修正） |
| 例6.2.10 | `Ex:computablefunctionevaluatable2` | \(f(2,3) \evaluate 5\) の導出例（今回導出修正） |
| 定義6.2.11 | `Def:computablefunction` | 計算可能関数の定義 |
| 補題6.2.12 | `lemma:evaluationderivationshape` | 導出は「(SUB)を尽くしてから(REPL)」の形に書ける |
| 定義6.2.13 | `Def:computablefunctionheadfunctionsymbol` | 主要関数記号 |
| 補題6.2.14 | `lemma:evaluationreplrestriction` | (REPL) で置換可能な部分項の制限 |
| 演習6.2.1 | `Que:basicfunctionscomputable` | ゼロ・後者・射影関数の計算可能性 |
| 演習6.2.2 | `Que:additioncomputable` | 加法の計算可能性（例6.2.9/6.2.10の完成） |
| 演習6.2.3 | `Que:fibonaccicomputable` | フィボナッチ数列の計算可能性 |
| 定理6.2.15 | `Thm:primitiverecursivefunctioncomputable` | 原始帰納的関数 ⇒ 計算可能関数 |
| 定理6.2.16 | `Thm:recursivefunctioncomputable` | 帰納的関数 ⇒ 計算可能関数 |

（番号は `src/contents/computable.aux` を最新のビルドで再生成した値．演習の削除により自動的に繰り上がっている．）

## 今後の課題（未着手）

- **Kleeneの標準形定理の証明**：現状は`hirose2024`を引用するのみで証明を与えていない．ユーザーは6.2節に続いてこの定理に向かう意向を示している．関数記号にアリティを要請しない設計判断は既にユーザーと議論済みで「問題なし」と確認しているが，\(T_n\), \(U\) の具体的な構成・証明はまだ書かれていない．
- **`Thm:recursivefunctioncomputable`の逆（計算可能関数 ⇒ 帰納的関数）**：現状「本書の範囲を超える」として扱っていない．将来的に扱うかどうかは未定．
- **6.2節以外でのREPL適用パターンの監査**：今回の監査は6.2節（`Def:computablefunctionterm`以降，`Thm:recursivefunctioncomputable`まで）に限定して行った．6.1節・6.3節以降，および他章に同種の問題がないかは未確認．
- **今後この節に手を入れる際の留意点**：演習6.2.2〜6.2.3や定理6.2.15〜6.2.16の証明は「\(x_1,\dots,x_n\)（や\(y\), \(b\)）を先に固定してから数項として扱い，(REPL)は必ずすべての引数が数項になってから適用する」という規律を徹底することで正しさを保っている．今後この節に追記・修正する際も同じ規律を維持する必要がある．
