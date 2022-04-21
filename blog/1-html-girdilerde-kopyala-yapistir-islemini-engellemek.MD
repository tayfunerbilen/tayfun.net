# HTML girdilerde kopyala-yapıştır işlemini engellemek

Kullanıcıdan bilgi almamız gereken durumlarda `<input />` ve `<textarea />` gibi form etiketlerini kullanıyoruz ve bazı durumlarda kullanıcıların bu etiketlere kopyala-yapıştır yapmasını engellemek isteyebilirsin.

Örneğin kredi kartının son 3 hanesinin girildiği bir senaryoda kullanıcının bunu doğrudan yapıştırmasını engellemek isteyebilirsin.
Ya da önemli bir veriyi normal kullanıcıların kopyalamayacağı şekilde ayarlamak isteyebilirsin. Normal kullanıcı diyorum çünkü geliştiriciler için ön tarafta yapılan önlemlerin çok bir önemi yok biliyorsun :)

Bu gibi durumlarda kopyalamayı engellemek için `oncopy`, yapıştırmayı engellemek için `onpaste` olaylarında işlem yapabilirsin.

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

Ayrıca her iki olayı aynı etikete koyarak kopyala-yapıştırı normal kullanıcılar için tamamen engellemiş oluruz.
