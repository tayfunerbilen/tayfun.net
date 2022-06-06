---
title: 'React ile Yazı Renklendirme'
date: '2022-06-06'
---

# React ile Yazı Renklendirme

React'de bazen bazı yazıları vurgulamak için farklı göstermeye ihtiyacınız olabilir. Örneğin içinde `javascript` geçen kelimeleri bulup onlara farklı bir stil işlemi uygulamak isteyebilirsiniz.

Bu gibi durumlarda en basit yöntem şu olabilir.

```js
export default function App() {
  const text = "lorem ipsum dollor amet.";
  const highlight = "lorem";

  return (
    <div
      dangerouslySetInnerHTML={{
        __html: text.replaceAll(
          new RegExp(`(${highlight})`, "gi"),
          "<mark>$1</mark>"
        )
      }}
    />
  );
}
```

Ancak burada bir sorun var, bu vurgulanan eleman artık bir JSX elemanı olmaktan çıkıyor. Dolayısı ile JSX'e uygulayabileceğiniz işlemleri buna uygulayamıyorsunuz.

O yüzden bu yaklaşımı bir component mantığına çevirip şu şekilde uygularsanız artık JSX olarak istediğiniz işlemleri uygulayabilirsiniz.

```js
function HighlightText({ text, highlight }) {
  let parts = text.split(new RegExp(`(${highlight})`, "gi"));
  return parts.map((part) =>
    part.toLocaleLowerCase("TR") === highlight.toLocaleLowerCase("TR") ? (
      <mark>{part}</mark>
    ) : (
      part
    )
  );
}

export default function App() {
  const text = "lorem ipsum dollor amet.";
  const highlight = "lorem";

  return <HighlightText text={text} highlight={highlight} />;
}
```

Örneğin hashtag ile başlayan etiketleri vurgulamak isteseydiniz de şu şekilde yapmanız yeterli olacaktı.

```js
function HighlightTags({ children, delimiter = '#' }) {
  let parts = children.split(new RegExp(`(${delimiter}[a-z]+)`, "gi"));
  return parts.map((part) =>
    part.includes(delimiter) ? (
      <mark>{part}</mark>
    ) : (
      part
    )
  );
}

export default function App() {
  const text = "lorem @ipsum #dollor amet.";
  return <HighlightTags>{text}</HighlightTags>;
}
```

Component'e `delimiter` ile özel bir ayraç belirleyip onu da kullanabilirsiniz. Örneğin `#` yerine `@` ile başlayanları vurgulamak isteseydiniz:

```js
export default function App() {
  const text = "lorem @ipsum #dollor amet.";
  return <HighlightTags delimiter="@">{text}</HighlightTags>;
}
```

Bu component'i biraz daha gelişmiş yazabiliriz, çünkü bu şekilde JSX elemanı bile olsa component haricinde bir onclick işlemi vs. yapamıyoruz.

```js
function Highlight({
  children,
  text = "",
  render,
  startsWith = null,
  match = "[a-z]+",
  includes = null
}) {
  let parts = (text || children).split(
    new RegExp(startsWith ? `(${startsWith}${match})` : `(${includes})`, "gi")
  );
  return parts.map((part) =>
    (startsWith ? part.startsWith(startsWith) : part.includes(includes))
      ? render(part)
      : part
  );
}

export default function App() {
  const clickTag = () => console.log("tag clicked");
  return (
    <>
      <Highlight
        text="bu yazı #tag örneği içerir."
        startsWith="#"
        render={(tag) => <mark onClick={clickTag}>{tag}</mark>}
      />
      <br />
      <Highlight
        text="bu yazı #türkçe tag örneği içerir."
        startsWith="#"
        match="[a-züçşğıİ]+"
        render={(tag) => <mark onClick={clickTag}>{tag}</mark>}
      />
      <br />
      <Highlight
        text="bu yazı @mention ile ilgil örnek içerir."
        startsWith="@"
        render={(mention) => <mark>{mention}</mark>}
      />
      <br />
      <Highlight
        includes="highlight"
        render={(tag) => <mark onClick={clickTag}>{tag}</mark>}
      >
        Bu örnekte ise herhangi bir kelimeyi highlight edebiliriz. Ve highlight
        edilen değerin kaç kere geçtiği önemsizdir.
      </Highlight>
    </>
  );
}
```

Artık kullanabileceğimiz 5 tane prom'umuz var.

- `render` - Vurgulanan elemanı render etmek için gerekli fonksiyonumuz. Burada istediğimiz etiket içinde yazdırabilir, onClick vs. gibi işlemler uygulayabiliriz.
- `startsWith` - Vurgulanacak değer özel bir değer ile başlayacaksa bunu belirtebilirsiniz. Örneğin # ya da @ karakterleri ile başlayanı vurgulamak istediğinizde.
- `includes` - Vurgulanacak değeri doğrudan belirleyip geçen yerleri vurgulatabilirsiniz.
- `match` - Vurgulanacak değer özel bir değerle başlıyorsa ve devamında ekstra bir regex'e ihtiyaç duyarsanız.
- `render` - Vurgulanacak değeri render etmek için gerekli fonksiyon.
