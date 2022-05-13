---
title: 'React forwardRef() Kullanımı'
date: '2022-05-11'
---

# React forwardRef() Kullanımı

React'de bir DOM elemanına ulaşmak için `useRef()` ile ref'lemek gerekiyor. Örneğin;

```js
import { useRef } from "react"

function App() {
  const ref = useRef()
  
  return (
    <>
      <input type="text" ref={ref} />
      <button onClick={() => ref.current.focus()}>Input'a Focusla</button>
    </>
  )
}
```

Ancak bu senaryoda input etiketi bir React Component'i olsaydı, bu şekilde bir refleme çalışmayacaktı. İşte bu noktada devreye `forwardRef()` giriyor. Alt component'den üst component'e refi iletmemizi sağlıyor.

Şimdi aynı örneği input'un component olduğu senaryoda yazalım.

```js
import { useRef, forwardRef } from "react"

const Input = forwardRef((props, ref) => {
  return (
    <input {...props} ref={ref} />
  )
})

function App() {

  const ref = useRef()
  
  return (
    <>
      <Input type="text" ref={ref} />
      <button onClick={() => ref.current.focus()}>Input'a Focusla</button>
    </>
  )
}
```

Artık `forwardRef()` ile ref'leri nasıl üst component'e ileteceğinizi biliyorsunuz. Tebrikler :)
