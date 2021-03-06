# Volt で Haml や Slim などを利用できますか？

**更新** - はい、Volt は Slim をサポートします。@ASnow ありがとう！ https://github.com/asnow/volt-slim

HAML はまだサポートされていません。以下は、関連する内容や必要なことを簡単に説明するものです。

テンプレート言語の追加が容易な Rails を使っていた人にとって、この質問は気になるところだと思います。Rails では、テンプレートのレンダリングというものは、コンテキストに利用するテンプレートやオブジェクトを見つける処理です。その結果生成されるものは **1つの文字列** です。

しかし Volt では、クライアントに送信するために、テンプレートはまず「コンパイルされたテンプレート」にレンダリングされます。テンプレートは _文字列にレンダリングされる_ わけではありません。_ターゲットに対して構成_ されます。Volt が現在サポートしているのは以下の2つのターゲットです。

- 実際の DOM
- 文字列 (タグ属性の中のテンプレートのパートのレンダリングや、E-mail、そしてサーバーサイドの html のため)

実際の DOM をレンダリングするとき、Volt はレンダリングされた DOM ノードの場所を探索します。したがって、それらのノードをレンダリングするためのデータに変更があった場合は、すべての DOM 全体を再レンダリングすることなく DOM を更新できます。これは、データ更新時の DOM に対する操作を最小限に抑え、DOM の更新を高速にすることに効果があります。他のフレームワークのように差分をチェックする必要もありません。Volt のバインディングは、DOM の更新を最小限の更新だけで行うことができます。

## では、Haml や Slim などをレンダリングするためには...

それは実現できないほど困難、というわけではありませんが、Haml や Slim の Gem はそのままでは Volt で使えるようには設計されていません。Volt のコンパイルされたテンプレートフォーマットにコンパイル可能なテンプレート言語はであればすべて利用することができます。その、コンパイルされたテンプレートフォーマットというのは Ruby のコードです (Opal 経由でクライアントにコンパイルされます)。それは HTML と バインディングから構成されています。HTML はプレースホルダー (現在、特殊なコメントとして実装) を持っており、バインディングがレンダリングされるべき場所を示しています。バインディングは Ruby の Proc であり、バインディングのインスタンスをセットアップします。(Volt::EachBinding, Volt::ContentBinding, Volt::AttributeBinding, Volt::IfBinding, and Volt::EventBinding)

Haml や Slim をコンパイルするためには、Haml や Slim が (レンダリング済みの HTML ではなく) Volt のコンパイルされたテンプレートのフォーマットを生成するように改修する必要があります。もし興味がある方がいれば、ぜひ [gitter](https://gitter.im/voltrb/volt) で @ryanstout までお知らせください。
