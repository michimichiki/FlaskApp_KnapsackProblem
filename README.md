# 最適なネットショッピング

## 概要
ネットショッピングなどで買い物をするとき、いろいろな商品が目に入ってしまい欲しいものがどんどん増えてしまうことはありませんか？このアプリケーションはたくさんの商品の中から満足度が高くなるような組み合わせを提案します。

## 使い方
1. app.pyを実行し、アプリを起動する。
2. 「商品を登録」から、商品名、値段、重要度を入力する。（重要度は主観的に決めた整数値を入力する。）
3. 欲しい商品を登録し終えたら「予算を設定」から、予算を入力する。
4. 「計算する」を押すとおすすめの商品が提案される。
    #### 補足事項
    * 商品の登録はいくつでも可能だが、多すぎると計算負荷が高くなり時間がかかる。
    * 登録した商品を削除したい場合は、「商品を削除」から商品名を入力すると削除可能。
    * 「リセット」を押すと入力したデータはすべて初期化される。

## 使用技術、フレームワーク／ライブラリ
* Python 3.11.1
* HTML
* Flask
* SQLite3
* numpy

<br><br>


# 組み合わせ最適化の手法
満足度の高い組み合わせは、遺伝的アルゴリズムという手法を用いた最適化計算によって提案される。以下では遺伝的アルゴリズムについて簡単に記載する。

## 遺伝的アルゴリズムの概要
遺伝的アルゴリズムとは、生物の進化の過程から着想を得た最適化手法である。生物は親が子を生み、それを繰り返すことで繁栄していく。その過程で、環境に適応できない個体は淘汰されていき、環境に適した生物は子孫を増やしていく。遺伝的アルゴリズムは、個体の適応度を評価し、適応度の高い個体を親としてその性質を引き継ぐ子個体を生成するという処理を繰り返し行うことで最適解を探索する。<br><br>
以下に簡単な手順を示す。
1. 初期個体をランダムにいくつか生成する。
2. 各個体の適応度を評価する。
3. 適応度の高い個体を選択する。
4. 選択された個体を親として、性質を引き継ぐような子個体をいくつか生成する。
5. 終了条件を満たすまで2~4を繰り返す。

## 商品の組み合わせ最適化
本アプリにおける最適化では、上記の個体は商品の組み合わせ、適応度は重要度の総和に相当し、以下のように設定している。
* 個体：登録した商品数の長さ分の配列。商品を組み合わせに含む場合は1に、含まない場合は0とする。<br>
    　（例）登録した商品が [ ガム, チョコ, アメ , グミ] で、ガムとグミが組み合わせとして選択される時、個体は [ 1, 0, 0, 1 ]

* 適応度：組み合わせに含まれる商品の重要度の総和。<br>
    　上の例で考えると、重要度を [ 3, 4, 2, 5 ] と設定していた場合は 3+5=8 が適応度となる。

最適化手順の4における子個体の作り方にはいくつか種類があるが、本アプリでは交叉（crossover）と突然変異（mutation）というものを使用している。<br>
交叉は親個体の一部の順列を入れ替えるような操作で、突然変異は一定確率で個体の一部をビット反転する操作である。ソースコード詳細は knapsack.py へ。
