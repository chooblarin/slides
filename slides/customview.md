class: center, middle, normal
## カスタムビューについて
(2014/12/08)

---
class: left, top, normal
### この発表について

- アメリカから持ち帰ってきた内容です

- 初心者向けな内容で初回にふさわしい

- 元ネタのスライドは[こちら](https://speakerdeck.com/smbarne/the-art-of-building-reusable-ui)

- GitHubに[サンプルコード](https://github.com/smbarne/AndroidReusableUI)もあがってる．やったぜ！

---
class: normal
### 概要

- 度々必要になるコンポーネントを再利用する方法

- 内部の実装をカプセル化して使いやすくしておこう

- デベロッパーフレンドリなAPI設計にしておこう

- 外部に公開するためのいろいろ
...

---
class: normal
### AndroidのUI

- Activity > Fragment > View

- スクリーンはなるべくFragmentでつくるのが[ベストプラクティス](https://github.com/futurice/android-best-practices)とされている

- ActivityはFragmentを制御し，FragmentはViewを制御するのが一般的な構造

- Viewレベルでよく使いそうなコンポーネントがあったらカスタムビューにしたい

---
class: normal
### レイアウトリソースファイルを使い回す

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="match_parent"
  android:layout_height="match_parent">

  ...

  <include layout="@layout_hoge"/>
  ...
  ```
  - レイアウトのみ再利用するなら`<include>`で十分
  - でも多分，大体同じViewロジックを書くことになる

---
class: normal
### カスタムViewをつくる

1. ViewやViewGroupを継承する

2. レイアウトXMLをつくる

3. Viewを操作するメソッドを追加する

以上！簡単〜

---
class: normal
### カスタムViewにしたいものってどんなもの

- ローディングインディケーターを表示するビュー (元ネタより)

- バッジアイコン付きボタン

- あるデータを決められたレイアウトに配置したいとか？

- 子Viewを先頭・末尾に追加したりするとか？

etc...

---
class: normal
### Viewの状態いろいろ
- ローディングアイコン付きのビューの例
- ローディング中
- コンテンツ表示
- コンテンツが無い（エラー画面なんかでもいい）

- バッジアイコン付きボタンの例
- 件数表示
- 最大数を超えたとき（99+とか）
- 0件のとき（バッジアイコンを非表示）

etc...

---
class: normal
### APIを定義 (ローディング付きビューの例)

```java
public void showLoading() {}
public void showContent() {}
public void showEmptyState() {}

public View setContentView(int contentViewResourceId) {}
public View setEmptyView(int messageResourceId, int imageDrawableResourceId) {}

```

- 機能を詰め込みすぎないように注意 (Do one thing very well)
- 初期化メソッドから使うものと同じにすると良い

---
class: normal
### APIを定義 (バッジアイコン付きボタンの例)

```java
public void setBadgeCount(int count) {}
public void reset() {}
```

とか…
多分実装はこんな

```java
public void setBadgeCount(int count) {
  badgeIcon.setVisibility(count > 0 ? VISIBLE : INVISIBLE);

  if (count > MAX_COUNT) {
    badgeIcon.setText("99+");
  }
}

public void reset() {
  badgeIcon.setText("");
}
```
---
class: normal
### XMLで使いたい

- XMLファイルで使うとき

```java
<com.chooblarin.ui.view.MyCustomView
  android:layout_width="match_parent"
  android:layout_height="wrap_content">
...

```

- アイコン画像リソースを変更したい
- メッセージテキストを設定したい

---
class: normal
### 属性を処理する (1)
- res/values/attrs.xmlに定義

```java
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <declare-styleable name="LoadingBananaPeelView">
    <attr name="bananaPeelDefaultMessageStringResource" format="reference" />
    <attr name="bananaPeelDefaultImageResource" format="reference" />
    <attr name="bananaPeelViewResource" format="reference" />
    <attr name="bananaPeelContentViewLayoutResource" format="reference" />
  </declare-styleable>
</resources>
```

- `name`には属性名を書く
- `format`には[AttributeFormat](https://code.google.com/p/idea-android/source/browse/trunk/src/org/jetbrains/android/dom/attrs/AttributeFormat.java?r=96)を書く
---
class: normal
### 属性を処理する (2)

```java
public LoadingBananaPeelView(Context context, AttributeSet attrs) {
  super(context, attrs);
  init(context, attrs);
}

private void init(Context context, AttributeSet attrs) {
  if (attrs != null) {
    TypedArray styledAttributes
    = getContext().getApplicationContext()
    .obtainStyledAttributes(attrs, R.styleable.LoadingBananaPeelView);

    // Load the resource IDs for each property from the styled attributes set
    bananaPeelContentViewLayoutResourceId
    = styledAttributes.getResourceId(
    R.styleable.LoadingBananaPeelView_bananaPeelContentViewLayoutResource, 0);
    bananaPeelViewResourceId
    = styledAttributes.getResourceId(
    R.styleable.LoadingBananaPeelView_bananaPeelViewResource, 0);

    styledAttributes.recycle();
  }
}
```

- `obtainStyledAttributes()`で`TypedArray`を取得する
- TypedArrayは共有リソースなので`recycle()`を呼ばないとメモリリークするらしいので注意
---
class: normal
### XMLから値を設定できるようになった

```java
<com.smb.loadingbananapeel.LoadingBananaPeelView
  xmlns:android="http://schemas.android.com/apk/res/android"
  xmlns:app="http://schemas.android.com/apk/res-auto"
  android:id="@+id/fragment_content_loading_view"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  app:bananaPeelDefaultImageResource="@drawable/ic_bananapeel_filled_default"
  app:bananaPeelDefaultMessageStringResource="@string/default_empty_message"
  app:bananaPeelContentViewLayoutResource="@layout/view_example_content" />
```


独自のスタイル属性を使うにはルートビューにネームスペースを定義してね
---

class: center, middle, normal

## 以上になります

(どうせ元ネタのスライドとサンプルコードがあるしな！)

---
class: center, middle, normal
# おまけ

---
class: normal
### カスタムビュー内のイベントを外で使うのはめんどくさい

- カスタムビューの外でクリックイベントを複数ハンドリングしたいときは

```java
interface MyCustomViewClickListener {
  void onLeftItemClick();
  void onCenterItemClick();
  void onRightItemClick();
  ...
}
```

みたくなっちゃうので注意．何をカスタムビューでつくるかはよく考えたい．

---
class: normal
## 作りまくると管理が大変

- 以前，気付いたらカスタムビューだらけになって大変になった

- 汎用的な部分を抽出して設計したい．

```
- view
|- MainMenuItemView
|- SubMenuItemView
|- SettingMenuItemView
|- SquaredImageView
|- SquaredLayout
|- SquaredThumbnailView
...
```
---
class: normal
## 最後に

サンプルコードのカスタムビュー，ViewFliperを継承してるのなんでだろう…
