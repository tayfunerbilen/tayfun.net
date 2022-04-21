# HTML girdilerde kopyala-yapıştır işlemini engellemek

Kullanıcıdan bilgi almamız gereken durumlarda `<input />` ve `<textarea />` gibi form etiketlerini kullanıyoruz ve bazı durumlarda kullanıcıların bu etiketlere kopyala-yapıştır yapmasını engellemek isteyebiliriz.

Örneğin kredi kartının son 3 hanesinin girildiği bir senaryoda kullanıcının bunu doğrudan yapıştırmasını engellemek isteyebiliriz.
Ya da önemli bir veriyi normal kullanıcıların kopyalamayacağı şekilde ayarlamak isteyebiliriz. Normal kullanıcı diyoruz çünkü geliştiriciler için ön tarafta yapılan önlemlerin çok bir önemi yok biliyorsunuz.

Bu gibi durumlarda kopyalamayı engellemek için `oncopy`, yapıştırmayı engellemek için `onpaste` eventlarında işlem yapabiliriz.

Yapıştırmayı engellediğimiz bir örnek:

```html
<!--
  bu input'a kopyalanan bir içerik yapıştırılamaz.
-->
<input type="text" name="cvv" placeholder="CVV" onpaste="return false" />
```

Kopyalamayı engellediğimiz bir örnek:

```html
<!--
  bu input'un içeriği kopyalanamaz.
-->
<input type="text" name="email" value="tayfun@erbilen.net" oncopy="return false" />
```

Ayrıca her iki event'ı aynı etikete koyarak kopyala-yapıştırı normal kullanıcılar için tamamen engelleyebilirsiniz.
