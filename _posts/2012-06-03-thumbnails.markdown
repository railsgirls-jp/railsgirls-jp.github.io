---
layout: default
title: アイデア一覧を表示したときにサムネイル表示してみよう
permalink: thumbnails
---

# Carrierwave を使ってサムネイルを作ってみよう

*Created by Miha Filej, [@mfilej](https://twitter.com/mfilej), Translated by Hiroshi SHIBATA*

__コーチへ__: 始めてのRailsアプリのStep4 でHTML の img タグの width を指定したことについて説明しましょう。また、それがサーバーで画像サイズを変更することの違いも話しあってみましょう。

## ImageMagickのインストール

* OS X: `brew install imagemagick` を実行します。もし `brew` コマンドが見つからない場合は[homebrewをインストール][in-homebrew]しましょう.
* Windows: [ImageMagick installer][im-win] をインストールします。(先頭にある *download* リンクをクリックします)
* Linux: Ubuntu か Debian を使っているなら `sudo apt-get install imagemagick` を実行します。他のディストリビューションを使っている場合は `apt-get` の代わりに適切なパッケージマネージャ(yum等)を使います。

  [im-win]: http://www.imagemagick.org/script/binary-releases.php?ImageMagick=vkv0r0at8sjl5qo91788rtuvs3#windows
  [in-homebrew]: http://mxcl.github.io/homebrew/

__コーチへ__: ImageMagick とは何をするものかを話しあいましょう。ImageMagick と gem(ライブラリ)と異なる部分や gem を追加する前に何故インストールしたかについても話してみましょう。

Rails アプリケーションの `Gemfile` を開いて

{% highlight ruby %}
gem 'carrierwave'
{% endhighlight %}

の下に

{% highlight ruby %}
gem 'mini_magick'
{% endhighlight %}

を追加します。その後に Terminal で以下のコマンドを実行します。

{% highlight sh %}
bundle
{% endhighlight %}

## 画像をアップロードした時にサムネイルを作成しよう

`app/uploaders/picture_uploader.rb` を開いて、以下の行を探しましょう。

{% highlight ruby %}
  # include CarrierWave::MiniMagick
{% endhighlight %}

上の行の `#` を削除します。

__コーチへ__: コード上のコメントの用途やどのような時に使うかを説明しましょう

削除した場所のすぐ下に以下を追加します。

{% highlight ruby %}
version :thumb do
  process :resize_to_fill => [50, 50]
end
{% endhighlight %}

これで画像をアップロードするとリサイズするようになりました。しかし、もうアップロード済みの画像は変更されません。もう一度 idea を編集して画像をアップロードし直してください。

## サムネイルを表示してみよう

`app/views/ideas/index.html.erb` を開いて、もし画像がアップロードされていればリサイズした画像を表示するようにしましょう。

{% highlight erb %}
<td><%= idea.picture %></td>
{% endhighlight %}

を下のように変更します。

{% highlight erb %}
<td><%= image_tag idea.picture_url(:thumb) if idea.picture? %></td>
{% endhighlight %}

これで、idea の一覧を開いた時にサムネイルが表示されるようになりました。