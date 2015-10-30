---
title: 他のフレームワークとの比較
type: guide
order: 19
---

## Angular

何にでも当てはまるわけではないと思いますが、Angular の代わりに Vue を使用する理由がいくつかあります：

- Vue.js は、API そして設計の両方の点から、Angular と比べて全体にとてもシンプルです。学習効率が非常に高いと言えます。

- Vue.js は、より柔軟で自己を主張しないライブラリと言えます。すべてにおいて Angular way を余儀なくされるのに対して、自分の好みのやり方でアプリを構築できます。本格的な SPA を作成せずとも、インターフェイス・レイヤーとしてページ内に軽度の機能実装を実現できるので、他のライブラリとの組み合わせを自由に選ぶ余地が生まれます。しかし、より多くのアーキテクチャ上の意思決定する責任があります。例えば、Vue.js のコアは、デフォルトでルーティングや ajax 機能が付属しておらず、そして通常は、外部モジュールバンドラを使用してアプリケーションを構築していると前提としています。これが、おそらく最も重要な違いと言えるでしょう。

- Vue.js は、dirty check を行わないため優れたパフォーマンスを持ち、より容易に最適化できます。Angular は、スコープに変更がある度、すべてのウォッチャを評価し直すため、ウォッチャが多く存在すると遅くなりがちです。そのうえ、いくつかのウォッチャのトリガが他のトリガを更新する場合、digest cycle は "安定化のため" に複数回実行しなければならない可能性があります。Angular の利用者は多くの場合 digest cycle を回避するために難解な技術に頼る必要があり、そしていくつかの状況では多数のウォッチャを伴うスコープを容易に最適化する方法は存在しません。Vue.js は非同期のキューイングによって透過性依存的追跡を監視するシステムを採用しているので、明らかな依存関係が無い限りすべての変更のトリガは独立して機能し、この問題に悩まされることはありません。唯一最適化のヒントとして `track-by` パラメータをリストの `v-for` の際に渡せます。

- Vue.js では、コンポーネントは独自の View とデータロジックを持つ自己完結型のユニットであり、その中でディレクティブはさまざまな DOM 操作のひも付け(カプセル化)を行うものとして、両者の役割を明確に分離しています。Angular では、両者の使い分けに多くの曖昧な点が存在しています。

興味深いことに、Vue で存在していないかなりの数の Angular 1 の問題は、Angular 2 でも設計上の決定によって対処されます。

## React

React.js と Vue.js は、どちらもリアクティブ＆コンポーザブルな View のコンポーネントを提供し、いくつかの類似性があります。もちろん、多くの違いも同様にあります。

まず、内部実装は根本的に違います。React のレンダリングは実際の DOM がどのような状態であるかのメモリ内表現した仮想 DOM を活用します。状態を変更するとき、React は仮想 DOM の完全な再レンダリングを行い、その差分を求めて、そして実際の DOM にパッチをします。

仮想 DOM のアプローチは、任意のタイミングで View を描画する機能的な方法を提供します。オブザーバーを利用せず、更新ごとにアプリケーション全体を再描画しているため、View はデータと常に同期がされていることが保証されます。これは、他の isomorphic JavaScript アプリケーションでも同様の可能性を与えることができます。

API は賢いですが、React (または JSX) における1つの問題は、描画関数は多くの場合に多数のロジックを伴い、むしろインターフェイスの視覚的な表現というよりもプログラムの一部のように見えることです。一部の開発者にとっては恩恵になると思いますが、私のようなデザイナーと開発者のハイブリッドにとっては、テンプレートを持つことでデザインと CSS をはるかに簡単に視覚的に捉えられるようにしてくれます。JSX に JavaScript のロジックを組み合わせるのは、私がコードをデザインに変換していくために必要としている視覚モデルの邪魔になります。対照的に、Vue.js は、軽量なデータバインディング DSL によりコストをかけており、そのために視覚的に構築可能なテンプレートと、ディレクティブとフィルタにカプセル化されたロジックを持っています。

React について別の問題を挙げるなら、DOM の更新が完全に仮想 DOM に委任されていることです。実際に、DOM を自分でコントロール**したい**とき、少しトリッキーです（理論的には出来ますが、React の思想に反する結果になります）。アドホックな DOM 操作を必要とするアプリケーションの場合、これがかなり厄介な制限になることになります。この面では、Vue.js はより柔軟性があり、例として [multiple FWA/Awwwards winning sites](https://github.com/vuejs/vue/wiki/Projects-Using-Vue.js#interactive-experiences) には Vue.js が組み込まれています。

Some additional notes:

- The React team has very ambitious goals in making React a platform-agnostic UI development paradigm, while Vue is focused on providing a pragmatic solution for the web.

- React, due to its functional nature, plays very well with functional programming patterns. However it also introduces a higher learning barrier for junior developers and beginners. Vue is much easier to pick up and get productive with in this regard.

- For large applications, the React community has been doing a lot of innovation in terms of state management solutions, e.g. Flux/Redux. Vue itself doesn't really address that problem (same for React core), but the state management patterns can be easily adopted for a similar architecture. I've seen users use Redux with Vue, and engineers at Optimizely have been using NuclearJS (their Flux dialect) with Vue as well.

- The trend in React development is pushing you to put everything in JavaScript, including your CSS. There has been many CSS-in-JS solutions out there but all more or less have its own problems. And most importantly, it deviates from the standard CSS authoring experience and makes it very awkward to leverage existing work in the CSS community. Vue's [single file components](http://vuejs.org/guide/application.html#Single_File_Components) gives you component-encapsulated CSS while still allowing you to use your pre-processors of choice.

## Ember

Ember は非常に独断的に設計されたフル機能フレームワークです。たくさんの確立された規約を提供し、一度それらに十分精通していると、非常に生産的に作ることができます。しかしながら、それは学習曲線が高いことと柔軟性が悪くなることを意味します。独断的なフレームワークと一緒に動作するツールの疎結合セットなライブラリとの間で選択するのを試すのはトレードオフです。後者はよりあなたに自由を与えるだけではなく、あなたによりアーキテクチャの意思決定をする必要があります。

以下は、恐らく Vue.js コアと Ember のテンプレートとオブジェクトモデルレイヤとの間のよりよい比較になるでしょう:

- Vue はプレーンな JavaScript オブジェクトでの控えめなリアクティブティ、そして完全に自動的な算出プロパティを提供します。Ember では、Ember オブジェクトで全てラップする必要があり、手動で算出プロパティの依存性を宣言する必要があります。

- Vue のテンプレート構文は JavaScript 式のフルパワーであり、Handlebars の式とヘルパー構文は比べてみるとかなり限定的です。

- Ember 2.0 の最新の Glimmer エンジン に更新した後で、公正な利ざやで比較しても、Vue は Ember よりもパフォーマンスがよいです。Ember ではパフォーマンスが重要な状況では、手動で実行ループを管理する必要がありながら、Vue は自動的にバッチ更新します。

## Polymer

Polymer はさらにもう1つの Google によってスポンサーされたプロジェクトで、実際には Vue.js も同様、インスピレーションの源でした。Vue.js のコンポーネントは Polymer のカスタム要素と比較して緩く、そして両方ともとても似た開発スタイルを提供します。最大の違いは、Polymer は最新の Web Components の機能に基づいて構築されており、そしてネイティブにこれらの機能をサポートしていないブラウザでは動作せるために(パフォーマンス低下)、ささいでない polyfills を必要します。これとは対照的に、Vue.js は IE9 まで依存せずに動作します。

また、Polymer 1.0 はパフォーマンスを保証するために非常に限定的なデータバインディングのシステムしか持たされていませんでした。例えば、Polymer のテンプレートでサポートされる唯一の式は、ブール否定と単一メソッド呼び出しです。そこでの算出プロパティの実装もまた非常に柔軟であるとは言えません。

最後に、プロダクションにデプロイする場合、Polymer の要素は vulcanizer と呼ばれる Polymer 依存のツールによってバンドルされる必要があります。一方、単一ファイルの Vue のコンポーネントは Webpack のエコシステムを提供しているすべてのものを活用でき、したがって Vue コンポーネントで ES6 や希望するあらゆる CSS プリプロセッサを簡単に利用できます。

## Riot

Riot 2.0 はコンポーネントベースの開発モデルに似たもの (Riot では "tag" と読んでいます) および、最小限の美しく設計された API を提供します。私は Riot と Vue は多くの設計哲学を共有していると考えます。しかし、Riot より若干重いにもかかわらず、Vue には Riot に対していくつかの重要な利点を提供します。

- 真に条件付きのレンダリング (Riot はすべての if のブランチをレンダリングし、単にそれらを表示/非表示にします)
- ずっとパワフルなルータ (Riot の ルーティング API は簡素すぎます)
- より成熟したツールのサポート (webpack と vue-loader をご覧ください)
- トランジションエフェクトシステム (Riot にはありません)
- よりよいパフォーマンス (Riot は実際には virtual-dom よりもダーティチェックを使うので、Angular と同様のパフォーマンスの問題をいくつか抱えています)