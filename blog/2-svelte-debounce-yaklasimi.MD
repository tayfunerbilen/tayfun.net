---
title: 'Svelte Debounce Yaklaşımı'
date: '2022-04-26'
---

# Svelte Debounce Yaklaşımı

Svelte'de kullanıcıdan bilgi alırken girilen bilgiyi state'e aktarmak için `bind:value` diyerek doğrudan state'in güncellenmesini sağlıyoruz.

```html
<script>
let search = ''
</script>

<input type="text" bind:value={search} /> <br />
Yazılan değer: {search}
```
demo: [https://svelte.dev/repl/25d143f7c81645d3bce2fa3560c57418?version=3.47.0](https://svelte.dev/repl/25d143f7c81645d3bce2fa3560c57418?version=3.47.0)

Ancak bazı durumlarda kullanıcının her yazdığı harfi anında güncellemesini değilde yazdıktan 300-400ms sonra bu state'in güncellenmesini isteyebilirsin. Bu durumua `debounce` diyoruz.

Bunu yapmanın farklı yolları var, kendimiz yazabileceğimiz gibi mevcut bir paketide kullanabiliriz. Örneğin yukarıdakini kendimiz yazacak olsak şöyle yapmalıydık.


```html
<script>
let search = '', timeout
const searchHandle = e => {
  clearTimeout(timeout)
  timeout = setTimeout(() => search = e.target.value, 300)
}
</script>

<input type="text" vale={search} on:input={searchHandle} /> <br />
Yazılan değer: {search}
```
demo: [https://svelte.dev/repl/5ba3c7b41e9d40b2a603a6ed50ab21aa?version=3.47.0](https://svelte.dev/repl/5ba3c7b41e9d40b2a603a6ed50ab21aa?version=3.47.0)

ya da benimde sıkça kullandığım [just-debounce-it](https://www.npmjs.com/package/just-debounce-it) paketini kullanarak yapabilirdik. Öncelikle paketimizi kuralım.

```sh
#npm için
npm -i just-debounce-it

# yarn için
yarn add just-debounce-it
```

Artık paketi import edip şöyle kullanabiliriz.

```html
<script>
import debounce from 'just-debounce-it';

let search = ''
const searchHandle = debounce(e => search = e.target.value, 300);
</script>

<input type="text" vale={search} on:input={searchHandle} /> <br />
Yazılan değer: {search}
```
demo: [https://svelte.dev/repl/17495840263348d39d9efe6e473b13ef?version=3.47.0](https://svelte.dev/repl/17495840263348d39d9efe6e473b13ef?version=3.47.0)

> Bunu genelde autocomplete yaptığınız aramalarda ve performansı artırmak için kullanabilirsin.
