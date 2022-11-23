---
title: 'React Excel Export'
date: '2022-11-23'
---

# React Excel Export

React ile bir array'i excel olarak export etmek isterseniz şu adımları izleyebilirsiniz:

1. Şu iki paketi kurun.
```bash
npm i file-saver sheetjs-style
```

2. Export işlemi için şöyle bir fonksiyon oluşturun:

```js
function exportArrayToExcel({ data, fileName }) {
	const fileType = 'application/vdn.openxmlformats-officedocument.spreadsheetml.sheet;charset=utf-8'
	const fileExtension = '.xlsx'
	const exportToExcel = () => {
		const ws = XLSX.utils.json_to_sheet(data)
		const wb = {
			Sheets: { data: ws, },
			SheetNames: ['data']
		}
		const excelBuffer = XLSX.write(wb, {
			bookType: 'xlsx',
			type: 'array'
		})
		const data = new Blob([excelBuffer], {
			type: fileType
		})
		FileSaver.saveAs(data, fileName + fileExtension)
	}

    return exportToExcel
}
```

3. Export etmek istediğiniz datayı formatlayıp şu şekilde fonksiyonu tetikleyin.

```js
// fonksiyonu nereye koyduysanız oradan çağırın
import exportArrayToExcel from "./export-array-to-excel"

export default function App() {
    const data = [
        {
            name: 'Tayfun',
            surname: 'Erbilen',
            age: 29
        },
        {
            name: 'Ahmet',
            surname: 'Günal',
            age: 24
        },
        {
            name: 'Mehmet',
            surname: 'Seven',
            age: 29
        }
    ]
    return (
        <div>
            İndir: 
            <button onClick={() => exportArrayToExcel(data, 'dosya adı')}>
                Export Excel
            </button>
        </div>
    )
}
```

Biraz paketleri kurcalayarak stil işlemleri ve bazı advanced kullanımlarıda hızlıca kavrayabilirsiniz :) En azından artık xlsx formatında bir dosyanız ve geçerli bir formatınız var. Bundan sonrasını sizin hayal gücü ve araştırmanıza bırakıyorum.