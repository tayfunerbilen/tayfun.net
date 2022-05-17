---
title: 'React useState() Hook'
date: '2022-05-17'
---

# React useState() Hook

React'de state oluşturmak için bu hook'u kullanıyoruz. Bize dizi olarak iki değer dönüyor. İlki oluşturulan değeri tutan değişken, diğeri ise bu değeri güncellemek için gerekli fonksiyon.

Oluşturduğunuz state'leri doğrudan güncellemek yerine, ikinci değerde döndürdüğü fonksiyon yardımıyla güncellemeniz gerekiyor.

Ayrıca `useState()` içinde o state'in başlangıç değerini belirleyebilirsiniz. Bu başlangıç değeri string, nesne, dizi gibi veri türlerini içerebilir.

Kısaca react'de bir state oluşturmak şöyle:

```jsx
import {useState} from "react"

function App() {
  const [name, setName] = useState('tayfun')
  return (
    <>
      <h3>{name}</h3>
      <button onClick={() => setName('ahmet')}></button>
    </>
  )
}
```

Ayrıca state'i güncellemek için kullanacağınız fonksiyon'a parametre olarak bir callback fonksiyonu tanımladığınızda bir önceki değerini alıp işlem yapabiliyorsunuz. Örneğin:

```jsx
import {useState} from "react"

function App() {
  const [counter, setCounter] = useState(0)
  return (
    <>
      <h3>Butona {counter} kez tıklandı.</h3>
      <button onClick={() => setCounter(prevValue => prevValue += 1)}>Tıkla</button>
    </>
  )
}
```
