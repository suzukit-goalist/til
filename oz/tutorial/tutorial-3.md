# クローリング

## パラメータ

http://urx3.nu/tUga

## cookbooks

### ページング方法

nextPageCheckTypeの設定値により、ページング方法が決まる。  
大きく分けて以下の2つの方法がある。

* URLに含まれる文字列をインクリメントして次ページへ遷移するURLを取得す
  る方法

* 次ページヘ遷移する用のaタグに含まれるURLを抽出する方法

### URLに含まれる文字列をインクリメントして次ページへ遷移する

* URLのどの部分をインクリメントさせるかを選択する

  例えば、以下のURLの-page__1の部分をインクリメントする場合、以下の設定にする  
  http://doda.jp/DodaFront/View/JobSearchList/j_pr__13/-oc__03L/-preBtn__1/-page__1/
  
  設定例
  
  ```
  nextPageCheckType=7
  nextPageParam=http://doda.jp/DodaFront/View/JobSearchList/j_pr__13/-oc__03L/-preBtn__1/-page__
  nextPageScPreStr=-page__
  ```

* 全件数と1ページ当たりに表示される件数を設定する

  searchCountCheckClassに全件数が表示されているエレメントのクラス属性を設定する  
  searchCountByPageに1ページ当たりに表示される件数を設定する
  
  設定例                  
  
  ```
  searchCountCheckClass=red_kensu
  searchCountByPage=50
  ```
  
### 詳細ページに遷移後、さらに別ページ(別タブ)をクロールする

crawlTypeを2に設定する
targetSecondPageUrlIncStrに別ページに遷移するURLに含まれる文字列を設定する

設定例

```
crawlType=2
targetSecondPageUrlIncStr=-tab__jd/-fm__jobdetail/-mpsc_sid__
```
