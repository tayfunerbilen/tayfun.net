---
title: 'React Component Yapısı'
date: '2022-05-23'
---

# React Component Yapısı

Component yapısı sadece react'e özgü bir şey değil. Bütün javascript frameworklerinde bulunan bu yapı sayesinde, ihtiyacımız olan her şeyi component'lere bölerek yeniden kullanılabilir hale getirebilir ve tekrardan kod yazmayı engelleyebiliriz.

Böylece yapıların tamamı component üzerine kurulduğunda, yönetmesi ve geliştirmesi de çok daha kolay bir hal alacaktır.

Ayrıca component'lere `prop` yani özellik geçerekte component'in nasıl davranacağını belirleyebiliriz. Böylece bir component birden fazla şekilde kullanılabilir. Ancak unutmayın ki, component'ler genel olarak tek bir amaca hizmet ederler.

Örneğin projenin bir UI kütüphanesini oluştururken, bu öğelerin her birini ayrı componentler olarak oluştururuz. Mesela `button` örneğini ele alalım.

Bir `Button` componentinin amacı, stile uygun olarak bir butonu göstermekten ibaret olmalıdır. `prop` yardımı ile en fazla butonunun şeklini değitirmeliyiz, ona ek özelliker atamak yerine fakrlı bir component'e bölerek geliştirme yapmak daha sağlıklı olacaktır.

Örneğin bir button'a bastığımızda altında bir dropdown açtırmak istediğimizde aslında burada iki farklı component'imiz var demektir. İlki az önce yukarıda da bahsettiğim `button` compoenenti, bir diğeri ise açılan kısım için hazırlayacağımız `dropdown` componenti.

O halde gelin, yazının bundan sonraki aşamalarında bu iki component'i oluşturarak component yapısını ve propları öğrenelim.

### Bir Component'in Yapısı

React'de bir component oluşturmak istediğinizde büyük harf ile başladığından emin olun. Zira react, büyük harfle başlayan fonksiyonları bir component gibi kabul edecektir. Bu yüzden bir fonksiyon oluşturmak istediğinizde bunu büyük harfle başlatmayın.

Gelin en basit bir react component'i nasıl oluşturulur görelim.

```jsx
// ./components/Button.js

function Button() {
  return (
    <button>Buton</button>
  )
}
export default Button
```

Yukarıda herhanhi bir prop'u olmayan, sabit bir buton component'i oluşturduk. Bu şekilde oluşturulan component'lerin pek bir anlamı yoktur, belki kodunuzu split etmek için kullanabilirsiniz ancak daha anlamlı bir component yapısı için bunu birazdan düzenleyeceğiz.

### Component'in Prop'ları

`prop`lar ya da özellikler bir fonksiyona gönderilen parametreler gibi düşünülebilir. Yukarıdaki oluşturduğumuz component'i çağırmak ve kullanmak istediğimizde şöyle bir yol uyguluyoruz.


```jsx
// ./App.js
import Button from "./components/Button.js"

function App() {
  return (
    <div>
      <h3>Component Yapısı</h3>
      <Button />
    </div>
  )
}
export default App
```

Şimdi bu component'in görevi buton oluşturmak olduğu için evvela buton'un içeriğini prop olarak göndermemiz gerekiyor. Hadi gelin, bunu şöyle değiştirelim.

```jsx
// ./App.js
import Button from "./components/Button.js"

function App() {
  return (
    <div>
      <h3>Component Yapısı</h3>
      <Button text="Test Button" />
    </div>
  )
}
export default App
```

Gördüğünüz gibi `anahtar="değer"` mantığında proplarımızı component'e geçebiliriz. Ancak sonuç hala değişmeyecektir. Çünkü component'de bu gönderilen prop'ları alıp işlememiz gerekiyor.

O zaman `./components/Button.js` dosyasını şöyle değiştirip tekrar test edelim.

```jsx
// ./components/Button.js

function Button(props) {
  return (
    <button>{props.text}</button>
  )
}
export default Button
```

Tekrar kontrol ettiğimizde artık gönderdiğimiz `text` prop'unun butonun içinde yazdığını görebiliriz. Elbette `props.text` diye ulaşmak yerine şöyle bir kullanımda mevcuttu.


```jsx
// ./components/Button.js

function Button({ text }) {
  return (
    <button>{text}</button>
  )
}
export default Button
```

`props` bir obje olduğu için javascript'de destructuring ile bunu parçalayıp içindeki değeri doğrudan değişken olarak kullanabiliriz. Böylece biraz daha anlamlı bir component yapısı oluşturmuş oluruz.

Bir başka prop daha geçip onu da nasıl alacağımıza bakalım.

```jsx
// ./App.js
import Button from "./components/Button.js"

function App() {
  return (
    <div>
      <h3>Component Yapısı</h3>
      <Button text="Test Button" />
      <Button text="Test Button" variant="success" />
    </div>
  )
}
export default App
```

Şimdi `variant` prop'una göre buton'un stil işlemlerini yazabiliriz.

```jsx
// ./components/Button.js

function Button({ text, variant = 'default' }) {
  return (
    <button className={variant}>{text}</button>
  )
}
export default Button
```

Gördüğünüz gibi `variant = "varsayılan değer"` şeklinde eğer bu prop gönderilmemiş ise biz bir varsayılan değer tanımlayarak kullandık. Ve gelen `variant` değerini doğrudan button'un class'ı olarak tanımladık.

Böylece css'de `button.default` ya da `button.success` ile ilgili bir stil yazdığımda doğrudan buna etki edecek ve variant'ı neyse button o şekilde gözükecektir.

Gelin bu stil işlemini `module css` kullanarak yapalım.

```css
// ./components/Button.module.css

.default {
  background: #eee;
}
.success {
  background: green;
  color: #fff;
}
```

Ve bu stilleri button component'ine uygulayalım.

```jsx
// ./components/Button.js
import styles from "./Button.module.css"

function Button({ text, variant = 'default' }) {
  return (
    <button className={styles[variant]}>{text}</button>
  )
}
export default Button
```

Artık çıktıya baktığımızda farklı renklerde 2 button göreceğiz. Başarıyla basitte olsa component mantığını ve prop'ları öğrendiğimizi düşünüyorum.

> Component'ler her zaman geriye bir şeyler döndürmek zorundadır, ancak her zaman prop almak zorunda değillerdir.

Son olarak `prop` içerisinde `children` değerini göstermek istiyorum. Biz buton component'ini kullanırken şöyle kullanıyoruz.

```jsx
<Button text="Test Button" variant="success" />
```

Ancak burada `text` propu yerine bu componenti etiket mantığnda tanımlayıp değeri şöyle gönderseydik ne olurdu?

```jsx
<Button variant="success">
  Test Button
</Button>
```

Ne yazık ki gönderilen değeri alamazdık. İşte bu yüzden component'i değiştirip `children` prop'unu kullanmamız gerekiyor.

```jsx
// ./components/Button.js
import styles from "./Button.module.css"

function Button({ children, variant = 'default' }) {
  return (
    <button className={styles[variant]}>{children}</button>
  )
}
export default Button
```

Artık sonuç yine başarılı olacaktır. `children` her component'de `props` altında bulunur. Etiket gibi kullandığımızda içine ne yazarsak hepsi `children` içine dahil olacaktır.

Gönül isterdi ki react'de de vue ya da svelte'de olduğu gibi slot mantığı olsun, componentler çok daha iyi yönetilsin ancak şimdilik böyle bir özelliğimiz yok :)

Şimdilik anlatacaklarım bu kadar, belki daha sonra makaleyi revize edebilirim. Hepinize iyi kodlamalar!
