---
title: 'React createElement() Kullanımı'
date: '2022-05-10'
---

# React createElement() Kullanımı

Bildiğiniz gibi react'de html değil JSX yazıyoruz. Kullanımı kolay olması açısından neredeyse her şey html ile aynı. Tek önemli fark, etiketlerde `class` niteliği yerine `className` kullanıyoruz. Ya da örneğin `onclick` yerine `onClick` kullanıyoruz vs.

JSX'i başka bir yazıda inceleriz, şimdilik bu kadarını bilsek yeterli olur sanırım. Peki JSX gibi güzel bir nimet varken neden `createElement()` API'si kullanılır? Aslında bunu bir örnekle açıklamak istiyorum.

Örneğin bir `Button` componentimiz olsun. Ve bu componentimiz bazı `prop` lar alsın. Nedir bunlar, örneğin `icon`, `iconPosition`, `size`, `theme` gibi bir butonda olabilecek temel özellikler.

```js
// ./components/Button.js
function Button({ children, icon = false, iconPosition: 'left', size = 'big', theme = 'default', ...props }) {
  return (
    <button className={classNames({
      "flex items-center justify-center gap-x-1.5 text-center font-medium px-3 transition-colors rounded border shadow": true,
      "h-9 text-sm": size === 'big',
      "h-7 text-xs": size === 'small',
      "bg-white text-black border-gray-200": theme === 'default',
      "bg-green-600 text-white border-green-800": theme === 'success',
      "bg-red-600 text-white border-red-800": theme === 'danger',
      "flex-row-reverse": iconPosition === 'right'
    })} {...props}>
      {icon}
      {children}
    </button>
  )
}

export default Button
```

Yukarıdaki component'in çalışması için bazı bağımlılıklara ihtiyacımız var. Öncelikle ben `tailwind` kullandığım için class'lı tanımlıyorum stil işlemlerini. Eğer daha doğru bir sonuç görmek isterseniz tailwind'i kurmanız gerekiyor. Kendi sitesinde bununla ilgili bir [yönlendirme](https://tailwindcss.com/docs/guides/create-react-app) var, onu takip edebilirsiniz. Bunu da daha kolay yönetmek için `classnames` paketini kullanıyorum. Bu paketi kurmak için;

```
npm install classnames
yarn add classnames
```

Artık component'i import edip denemeler yapabiliriz.

```js
import Button from "./components/Button"
import {BiCheck} from "react-icons/bi"

function App() {
  return (
    <>
      <Button>Tayfun Erbilen</Button>
      <Button size="small">Tayfun Erbilen</Button>
      <Button theme="success" icon={<BiCheck size={16} />}>Başarılı</Button>
      <Button theme="success" icon={<BiCheck size={16} />} iconPosition="right">Başarılı</Button>
    <(>
  )
}
```

> **Not:** İkonlar için `react-icons` paketini kullanıyorum. Dilerseniz onu da şöyle kurabilirsiniz.
> `npm install react-icons` ya da yarn kullanıyorsanız `yarn add react-icons`

Şimdi gelelim sorumuza, biz bu component'i `button` etiketi yerine `a` etiketi ile nasıl değiştiririz? Bazı yerlerde `button` bazı yerlerde `a` etiketine ihtiyacımız var? Hatta bazı yerlerde, `react-router-dom` paketinin `NavLink` componentiyle kullanmamız gerekiyor?

İşte bu noktada, bu component'i `createElement()` API ile oluşturarak daha fazla özelleştirme yapabiliriz. Hadi gelin, aynı component'i tekrar `createElement()` ile yazalım.

Temelde `createElement()` şu yapıdadır;

```js
import { createElement } from "react"

function Button() {
  return createElement('button', {
    className: 'button'
  }, 'Ben Butonum')
}
```

Yani 3 parametre alıyor, etiketin adı, prop'lar ve text. Şimdi bunu bizim componentimize uyarlayalım.

```js
// ./components/Button.js
import { createElement } from "react"

function Button({ children, as = 'button', icon = false, iconPosition: 'left', size = 'big', theme = 'default', ...props }) {
  return createElement(as, {
    className: classNames({
      "flex items-center justify-center gap-x-1.5 text-center font-medium px-3 transition-colors rounded border shadow": true,
      "h-9 text-sm": size === 'big',
      "h-7 text-xs": size === 'small',
      "bg-white text-black border-gray-200": theme === 'default',
      "bg-green-600 text-white border-green-800": theme === 'success',
      "bg-red-600 text-white border-red-800": theme === 'danger',
      "flex-row-reverse": iconPosition === 'right'
    }),
    ...props
  }, children)
}

export default Button
```

Artık component'e prop olarak `as` altında istediğimiz etiketi ya da component'i belirterek component'in nasıl render olacağına karar verebiliriz.

```js
import Button from "./components/Button"
import {BiCheck} from "react-icons/bi"
import {NavLink} from "react-router-dom"

function App() {
  return (
    <>
      <Button>Tayfun Erbilen</Button>
      <Button as="a" href="https://prototurk.com" target="_blank" size="small">Prototürk'e Git</Button>
      <Button as={NavLink} to="/" theme="success" icon={<BiCheck size={16} />}>Geri Dön</Button>
    <(>
  )
}
```
