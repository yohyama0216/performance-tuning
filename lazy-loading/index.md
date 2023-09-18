リソースの遅延読み込み (Lazy loading)

リソースの遅延読み込みは、ページの読み込み時間を短縮し、初回表示時のユーザーエクスペリエンスを向上させるテクニックです。特に画像や動画といった大きなファイルに有効です。

HTMLのloading属性を利用
最もシンプルな方法として、HTMLのimgやiframeタグにloading="lazy"属性を追加する方法があります。

html
Copy code
<img src="example.jpg" loading="lazy" alt="Example Image">
Intersection Observer APIの利用
JavaScriptでより柔軟に遅延読み込みを制御する場合、Intersection Observer APIを利用できます。

javascript
Copy code
const observer = new IntersectionObserver((entries, observer) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;
      observer.unobserve(img);
    }
  });
});

document.querySelectorAll('img[data-src]').forEach(img => {
  observer.observe(img);
});
HTML側で、遅延読み込みをしたい画像には以下のようにdata-src属性を追加します。

html
Copy code
<img data-src="example.jpg" alt="Example Image">
JavaScriptライブラリの使用
数々のJavaScriptライブラリがLazy loadingのために提供されています。例としては、lazysizesやlozad.jsなどが挙げられます。

javascript
Copy code
// lazysizesの場合
import 'lazysizes';
対応するHTMLは以下のようになります。

html
Copy code
<img data-src="example.jpg" class="lazyload" alt="Example Image">
注意点
プレースホルダー: 遅延読み込みをすると、画像が表示されるまでの間に空白が表示されることがあるため、適切なサイズと色のプレースホルダーを利用するとよい。
SEO: 検索エンジンがコンテンツをクロールする際に、遅延読み込みが適切に行われているか確認が必要です。
