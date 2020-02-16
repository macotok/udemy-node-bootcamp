# udemy-node-bootcamp

ライブラリ
- express

コードフォーマット
- prettier
- eslint

## mongoコマンド

- database作成
- database切り替え

```terminal
> use natours-test
> use admin
```

- collection(tours)を作成
- documentを挿入

```terminal
> db.tours.insertOne({ name: "The Forest Hiker", price: 297, rating: 4.7 })
{
	"acknowledged" : true,
	"insertedId" : ObjectId("5e48e044637969c6f6dacc53")
}
```

- collectionのdataを表示

```terminal
> db.tours.find()
{ "_id" : ObjectId("5e48e044637969c6f6dacc53"), "name" : "The Forest Hiker", "price" : 297, "rating" : 4.7 }
```

- database一覧を表示

```terminal
> show dbs
admin         0.000GB
config        0.000GB
local         0.000GB
natours-test  0.000GB
```

- collection一覧を表示

```terminal
> show collections
tours
```
