class: center, middle

# Mastering ViewPager

---
class: left, top

# ViewPager

- スワイプで画面を切り替えるUIに使うアレ
- android.support.v4にある

## 使い方をおさらい

1. Activity（またはFragment）のレイアウトにViewPagerを追加する
2. PagerAdapterをつくる
3. `ViewPager#setAdapter()`でPagerAdapterをセット

--

## PagerAdapter

- ViewPagerはFragmentの切り替えに使うケースが最も多い（と思う...）
- 2種類のPagerAdapterが提供されている
  - [FragmentPagerAdapter](http://developer.android.com/reference/android/support/v4/app/FragmentPagerAdapter.html)
  - [FragmentStatePagerAdapter](http://developer.android.com/reference/android/support/v4/app/FragmentStatePagerAdapter.html)

- 2つの違いは，Fragmentのインスタンスを保持するか破棄するか．
- ページ数が多いときは`FragmentStatePagerAdapter`を使うとメモリにやさしい

--

## 