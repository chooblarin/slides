class: center, middle, inverse

# Elixirはじめの一歩 (前篇)
@chooblarin

2015/07/02
---

class: normal

## Agenda

1. イントロダクション

2. 型

3. 演算子

4. パターンマッチ

5. 制御構文　(case, cond, if)

6. 連想配列
---

class: normal

## イントロダクション

- ElixirはErlangのVM上で動く言語

- Elixirが流行るかもと[話題に](http://qiita.com/HirofumiTamori/items/0dfdbada30c7d8f183fd)

- 試しに触ってみたい．みんなでチュートリアルを一緒にやりましょう

というわけで[公式サイト](http://elixir-lang.org/install.html)の手順にしたがってインストール

---

class: normal

### Hello, world

```remark
$ elixir -v
Elixir 1.0.4
```

インストール完了です．`iex`コマンドを実行するとインタプリタが立ち上がる．
当面の間はこれを使う．

```
IO.puts "Hello, world"
```

これがHello, worldのコード．実行すると

```
Hello, world
:ok
```

こうなる．

---

class: normal

## 型

- 整数

- 浮動小数点数

- 文字列

- 関数

- List

- Tuple

- Atom

- (ブール値)

- (nil)

---

class: normal

### 文字列

次回に回します

---

class: normal

### List

```
iex> [1, 2, true, 3]
[1, 2, true, 3]
```

リストは任意の型の値入れられる．

```
iex> [1, 2, 3] ++ [4, 5, 6]
[1, 2, 3, 4, 5, 6]
```

連結ができて

```
iex> [1, true, 2, false, 3, true] -- [true, false]
[1, 2, 3, true]
```

こんな演算も出来る．先頭から該当する要素を取り除く操作．（詳細は後ほど）

---

class: normal

### Tuple

```
iex> tuple = {:ok, "hello"}
{:ok, "hello"}
```

タプルもリストと同様，どんな型の値でも保持出来る．

```
iex> elem(tuple, 1)
"hello"
```

```
iex> put_elem(tuple, 1, "world")
{:ok, "world"}
iex> tuple
{:ok, "hello"}
```

`put_elem/3`は新しくタプルを生成して返す．`tuple`は変更されない(Immutable)．

---

class: normal

### 無名関数

```
iex> add = fn a, b -> a + b end
#Function<12.90072148/2 in :erl_eval.expr/5>
iex> add.(1, 2)
3
```

無名関数はファーストクラスデータ型．

呼び出しには関数名と括弧の間に `.` が必要．

```
add_two = fn a -> add.(a, 2) end
#Function<6.90072148/1 in :erl_eval.expr/5>
iex> add_two.(2)
4
```

無名関数はクロージャなので，スコープ内の変数にアクセス出来る．

---

class: normal

## 演算子

- 四則演算 : `+`, `-`, `*`, `/`

- 整数の商と剰余の計算 : `div/2`, `rem/2`

- リスト操作 : `++`, `--`

- 文字列の連結 : `<>`

- ブール演算子 : `or`, `and`, `not`, `||`, `&&`, `!`

- 比較 : `==`, `!=`, `===`, `!==`, `<=`, `>=`, `<`, `>`

---

class: normal

### ブール演算子 (1)
`or`, `and`, `not` は第1引数にブーリアン型( `true` or `false` )以外を渡されるとエラーになる．

```
iex> 1 and true
** (ArgumentError) argument error
iex> 1 && true
true
```

`||`, `&&`, `!` は任意の型を受け付けることが出来る．`false` と `nil` 以外の全ての値がtrueと評価される．
```
iex> false || 11
11
iex> true && 17
17
iex> !nil
true
```

---

class: normal

### ブール演算子 (2)
`or` と `and` は（ `||` と `&&` も）ショートサーキット演算子．必要な部分だけしか評価しない．


```
iex> false and error("This error will never be raised")
false

iex> true or error("This error will never be raised")
true
```

```
iex> false && error("This error will never be raised")
false

iex> true || error("This error will never be raised")
true
```

---

class: normal

## パターンマッチ
`=` は代入演算子では無く，パターンマッチ演算子．Elixirに代入は必要無い．全ての変数がImmutableだから．

```
iex> {a, b, c} = {:hello, "world", 42}
{:hello, "world", 42}
iex> a
:hello
iex> b
"world"
```

パターンマッチも詳細は後ほど

---

class: center, middle, normal

## 制御構文　(case, cond, if)

---

class: normal

### case
case式はパターンマッチするまで上から順番に調べて，パターンマッチが成功した句を評価する．

```
result = case {1, 2, 3} do
  {4, 5, 6} ->
    "This clause won't match"
  {1, x, 3} ->
    "This clause will match and bind x to #{x} in this clause"
  _ ->
    "This clause would match any value"
end

IO.puts result
```

実行結果
```
This clause will match and bind x to 2 in this clause
```

---

class: normal

### cond
condは真偽値の評価結果で処理を分岐したいときに利用する．
```
result = cond do
  2 + 2 == 5 ->
    "This will not be true"
  2 * 2 == 3 ->
    "Nor this"
  1 + 1 == 2 ->
    "But this will"
end

IO.puts result
```

実行結果
```
But this will
```

---

class: normal

### if
ifも期待通り動作する．

```
result = if nil do
    "This won't be seen"
  else
    "This will"
  end

IO.puts result
```

実行結果
```
This will
```
---

class: center, middle, normal

## 連想配列

---

class: normal

### キーワードリスト
2-タプルのリストを*キーワードリスト*という．このタプルの第1要素はAtom型である必要がある．
省略記法も用意されている．

```
iex> list = [{:a, 1}, {:b, 2}]
[a: 1, b: 2]
iex> list == [a: 1, b: 2]
true
iex> list[:a]
1
```

リストと同様に値を追加出来る．

```
iex> new_list = [a: 0] ++ list
[a: 0, a: 1, b: 2]
iex> new_list[:a]
0
```

同じキーの値がある場合，先頭から優先してアクセスされる．

---

class: normal

### マップ

```
iex> map = %{:a => 1, 2 => :b}
%{2 => :b, :a => 1}
iex> map[:a]
1
iex> map[2]
:b
iex> map[:c]
nil
```

キーワードリストと違って，任意の値をキーに出来る．

---

class: center, middle, inverse

## 続きは次回
