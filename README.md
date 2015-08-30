% ElixirとPhoenixの紹介
% ka ([kaosfield](http://www.kaosfield.net))
% 2015-08-30

# ElixirとPhoenix

## Elixir

Erlang VMをRuby風の文法の言語で扱える

関数型の潮流を大きく汲んでいる

少数のデータ型と多数の関数

不変性・参照透過性

## Phoenix

Elixir on Rails

WAP

Version 1.0.0が一昨日出た

# Erlang

Actorモデルを採用したVM上で動作するプログラムを記述する言語

## **Let it crash**の思想

正常系(とビジネスロジックでの例外)だけ考えればいい

外部やDBとの通信エラー時のリトライ等の戦略はSupervisorに任せる

# 昨今のWEB Applicationの要件

ViewはJSON APIを用意してもらってフロントエンドが勝手にやる

HTML, CSS, JavaScriptは静的なコンテンツを素早く返す(動的にHTMLをリクエスト毎に生成するのは辛い)

サーバサイドはJSON吐き出し飲み込みする機械に特化する

JSON APIへのリクエストの頻度が高い

リアルタイム通信(HTTP2, WebSocket, WebRTC)も必要

<div class="incremental">
<span style="font-size:150%;color:red">その目的にElixir/Phoenixが特化してます</span>
</div>

# 採用事例

<iframe src="//www.slideshare.net/slideshow/embed_code/key/EmUX8O7KV4448z" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/ohr486/shibuyaex-1-elixir" title="Shibuya.ex #1 Elixirを本番環境で使ってみたという事例紹介" target="_blank">Shibuya.ex #1 Elixirを本番環境で使ってみたという事例紹介</a> </strong> from <strong><a href="//www.slideshare.net/ohr486" target="_blank">Tsunenori Oohara</a></strong> </div>

# Erlang VMの性能

> Whatsapp1は何億人ものユーザーを抱えています。他の大半のプラットフォームでは1万以上の同時コネクションを1台のマシンで処理するのはチャレンジングだと見られていますが、Whatsappは1サーバーあたり2百万の同時コネクションをこなしています。2百万コネクションをErlangが走る1台のサーバーで。

[[翻訳] Elixir - 次に来る大物Web言語 - Qiita](http://qiita.com/HirofumiTamori/items/0dfdbada30c7d8f183fd)

# パフォーマンス

[Elixir/cowboyとRailsのサーバーベンチマーク比較 - Qiita](http://qiita.com/ohr486/items/a6bf071f1fe26f5108ab)

引用:

|   |Rails|Elixir|
|:--|-----|-----:|
|(Failedが出ない)最大同時接続数|510|4900|
|最大同時接続時のRequest/Sec|722|4175|
|最大同時接続時のTime/Req(1リクエストあたり)|1.38ms|0.37ms|

# パフォーマンス

[mroth/phoenix-showdown](https://github.com/mroth/phoenix-showdown#comparative-benchmark-numbers)

via. [Phoenix Framework - Channel 日本語翻訳 - Qiita](http://qiita.com/niku/items/e846c4cbb9f1d15830cc)

引用:

| Framework      | Throughput (req/s) | Latency (ms) | Consistency (σ ms) |
| :------------- | -----------------: | -----------: | -----------------: |
| Phoenix        |          43063.45  |        2.82  |              7.46  |
| Sinatra        |           9182.86  |        6.55  |              3.03  |
| Rails          |           3274.81  |       17.25  |              6.88  |

# リアルタイム通信

[Phoenix Framework - Channel 日本語翻訳 - Qiita](http://qiita.com/niku/items/e846c4cbb9f1d15830cc)

[次の10年のためにElixirをハックする - SSSSLIDE](http://sssslide.com/speakerdeck.com/mizchi/ci-false10nian-falsetamenielixirwohatukusuru)

# 参考資料

[Shibuya.ex #1 のメモ - Time's up, let's do this!](http://shishi.hatenablog.jp/entry/2015/08/25/213432)

Shibuya.exでのスライドまとめになっている

#

ここから少しElixirのRubyとは毛色の違う文法紹介

# パターンマッチ

```
x = 1
1 = x # OK
```

```
x = 1
2 = x # ここでエラー(MatchError)になる
```

代入をしていると考えるのではなく1にxを束縛していると考えると分かりやすいかも

# パターンマッチ

```
f = fn
  0, 0, _ -> "FizzBuzz"
  0, _, _ -> "Fizz"
  _, 0, _ -> "Buzz"
  _, _, n -> n
end

f.(0, 0, 1) #=> "FizzBuzz"
f.(0, 1, 1) #=> "Fizz"
f.(1, 0, 1) #=> "Buzz"
f.(1, 1, 2) #=> 2
```

# パイプライン演算子

```
List.flatten [1, [2], 3] #=> [1, 2, 3]
Enum.map [1, 2, 3], &(&1 * 2) #=> [2, 4, 6]

[1, [2], 3] |> List.flatten |> Enum.map(&(&1 * 2)) #=> [2, 4, 6]
```

引用元: [Elixir - パイプライン演算子のはなし - Qiita](http://qiita.com/mururu/items/c6181a4e64dc182f9ad9)

インスパイア元はF#

Clojureの`->`マクロに似ている

#

おわり
