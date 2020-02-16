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

- documentを複数挿入

```terminal
> db.tours.insertMany([{ name: "The Sea Explorer", price: 497, rating: 4.8 }, { name: "The show Adventurer", price: 997, rating: 4.9, difficulty: "easy" }])
{
	"acknowledged" : true,
	"insertedIds" : [
		ObjectId("5e496c28637969c6f6dacc55"),
		ObjectId("5e496c28637969c6f6dacc56")
	]
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
