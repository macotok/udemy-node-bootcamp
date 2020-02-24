# udemy-node-bootcamp

ライブラリ
- express

コードフォーマット
- prettier
- eslint

mongoDBのGUI
- MongoDB Compass

## mongoコマンド

- database作成
- database切り替え
- `use`を使用

```terminal
> use natours-test
> use admin
```

- database一覧を表示
- `show`を使用

```terminal
> show dbs
admin         0.000GB
config        0.000GB
local         0.000GB
natours-test  0.000GB
```

- collection一覧を表示
- `show`を使用

```terminal
> show collections
tours
```

- collection(tours)を作成
- documentを挿入
- `insertOne`を使用

```terminal
> db.tours.insertOne({ name: "The Forest Hiker", price: 297, rating: 4.7 })
{
	"acknowledged" : true,
	"insertedId" : ObjectId("5e48e044637969c6f6dacc53")
}
```

- documentを複数挿入
- `insertMany`を使用

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

- collectionのdocumentを表示
- `find`を使用

```terminal
> db.tours.find()
{ "_id" : ObjectId("5e48e044637969c6f6dacc53"), "name" : "The Forest Hiker", "price" : 297, "rating" : 4.7 }
```

- collectionのdocument抽出
- `find`を使用

```terminal
> db.tours.find({ name: "The Forest Hiker"})
{ "_id" : ObjectId("5e48e044637969c6f6dacc53"), "name" : "The Forest Hiker", "price" : 297, "rating" : 4.7 }
```

- collectionの数値以内(=含む)のdocument抽出
- `find`、`$lte`を使用

```terminal
> db.tours.find({ price: { $lte: 297 } })
{ "_id" : ObjectId("5e48e044637969c6f6dacc53"), "name" : "The Forest Hiker", "price" : 297, "rating" : 4.7 }
```

- collectionの数値未満のdocument抽出
- `find`、`$lt`を使用

```terminal
> db.tours.find({ price: { $lt: 300 } })
{ "_id" : ObjectId("5e48e044637969c6f6dacc53"), "name" : "The Forest Hiker", "price" : 297, "rating" : 4.7 }
```

- collectionの数値未満かつ数値以上の複数条件のdocument抽出
- `find`、`$lt`、`$gte`を使用

```terminal
> db.tours.find({ price: { $lt: 500 }, rating: { $gte: 4.8 } })
{ "_id" : ObjectId("5e496c11637969c6f6dacc54"), "name" : "The Sea Explorer", "price" : 497, "rating" : 4.8 }
```

- collectionのORの複数条件のdocument抽出
- `find`、`$or`を使用

```terminal
> db.tours.find({ $or: [{ price: { $lt: 500 } }, { rating: { $gte: 4.8 } }] })
{ "_id" : ObjectId("5e48e044637969c6f6dacc53"), "name" : "The Forest Hiker", "price" : 297, "rating" : 4.7 }
{ "_id" : ObjectId("5e496c28637969c6f6dacc55"), "name" : "The Sea Explorer", "price" : 497, "rating" : 4.8 }
{ "_id" : ObjectId("5e496c28637969c6f6dacc56"), "name" : "The show Adventurer", "price" : 997, "rating" : 4.9, "difficulty" : "easy" }
```

- collectionで指定のfiled(name)のみ抽出({ name: 1 })

```terminal
> db.tours.find({ $or: [{ price: { $gt: 500 } }, { rating: { $gte: 4.8 } } ] }, { name: 1 })
{ "_id" : ObjectId("5e496c11637969c6f6dacc54"), "name" : "The Sea Explorer" }
{ "_id" : ObjectId("5e496c28637969c6f6dacc56"), "name" : "The show Adventurer" }
```

- collectionで1つのdocumentをupdateOne({ 対象のdata }, { $set: { 更新data } })で更新
- `updateOne`を使用

```terminal
> db.tours.updateOne({ name: "The show Adventurer"}, { $set: { price: 597 } })
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
```

- collectionで複数のdocumentをupdateMany({ 対象のdata }, { $set: { 更新data } })で更新
- `updateMany`を使用

```terminal
> db.tours.updateMany({ price: { $gte: 400 }, rating: { $gte: 4.8 } }, { $set: { premium: true } })
{ "acknowledged" : true, "matchedCount" : 3, "modifiedCount" : 3 }
```

- collectionで複数のdocumentをdeleteMany({ 対象のdata })で削除
- `deleteMany`を使用

```terminal
> db.tours.deleteMany({ rating: { $lt: 4.8 } })
{ "acknowledged" : true, "deletedCount" : 1 }
```

- collectionの全てのdocumentを削除
- `deleteMany`を使用

```terminal
> db.tours.deleteMany({})
{ "acknowledged" : true, "deletedCount" : 3 }
```

## mongoDBのGUI

- mongoDB compassを使用

## mongoDBをremoteで使う

- mongoDB Atlasで管理する
- DBはmongoDB Compassを使用してAtlasと接続
- localの作業環境でAtlasと接続

## mongoose

- mongoDBとNode.jsアプリケーションの中間レイヤー
- mongoDBを操作するためのライブラリ

### mongoDBとの接続

```javascript
const mongoose = require('mongoose');
const DB = process.env.DATABASE; // Atlasで設定されたURL

mongoose
  .connect(DB, {
    useNewUrlParser: true, // ローカルで実行されているデータベースをデフォルトポート(27017)で接続するために最低限必要な設定
    useCreateIndex: true, // Model.initを介した自動インデックス構築にensureIndexではなくcreateIndexを使用
    useFindAndModify: false // findOneAndUpdate、およびfindOneAndRemoveがfindAndModifyではなくネイティブfindOneAndUpdateを使用
  })
  .then(() => console.log('DB connection successful'));
```

### documentのschema設定

- 型定義
- 必須とエラーメッセージを設定
- 初期値設定
- 一意を設定

```javascript
const tourSchema = new mongoose.Schema({
  name: {
    type: String, // 型定義
    required: [true, 'A tour must have a name'], // 必須とエラーメッセージを設定
    unique: true // 一意を設定
  },
  rating: {
    type: Number,
    default: 4.5 // 初期値設定
  },
  price: Number // 型のみ定義
});

const Tour = mongoose.model('Tour', tourSchema); // Model名の頭文字は大文字
```

### document追加

- modelとschemaを定義したTour classの引数にdataを挿入
- saveメソッドはpromiseを返すのでエラーハンドリングも行う
- saveメソッドは古いversion。後述のcreateメソッドを使う

```javascript
const testTour = new Tour({
  name: 'The Forest Hiker',
  rating: 4.7,
  price: 997
});

testTour
  .save()
  .then(doc => {
    console.log(doc);
  })
  .catch(err => {
    console.log('ERROR:', err);
  });
```

- createメソッドでrequest dataを扱う
- async awaitでdata取得
- try catch文でエラーハンドリグ

```javascript
exports.createTour = async (req, res) => {
  try {
    const newTour = await Tour.create(req.body);
    res.status(201).json({
      status: 'success',
      data: {
        tour: newTour
      }
    });
  } catch (err) {
    res.status(400).json({
      status: 'fail',
      message: 'Invalid data sent'
    });
  }
};
```

### collectionの全document取得

- async awaitでdata取得
- findメソッドでcollectionの全document取得
- try、catch文でエラーハンドリグ

```javascript
exports.getAllTours = async (req, res) => {
  try {
    const tours = await Tour.find();

    res.status(200).json({
      status: 'success',
      results: tours.length,
      data: {
        tours
      }
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err
    });
  }
};
``` 

### collectionで指定したIDのdocument取得

- async awaitでdata取得
- findByIdメソッドの引数に取得したいidを指定
- try、catch文でエラーハンドリグ

```javascript
exports.getTour = async (req, res) => {
  try {
    const tour = await Tour.findById(req.params.id);

    res.status(200).json({
      status: 'success',
      data: {
        tour
      }
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err
    });
  }
};
```

### collectionで指定したIDのdocument更新

- async awaitでdata取得
- findByIdAndUpdateメソッドの引数(更新したいidを指定、更新内容、option)を指定
- try、catch文でエラーハンドリグ

```javascript
exports.updateTour = async (req, res) => {
  try {
    const tour = await Tour.findByIdAndUpdate(req.params.id, req.body, {
      new: true, // オリジナルではなく変更されたドキュメントを返す場合はtrue
      runValidators: true // trueの場合、このコマンドで更新バリデーターを実行します。Modelのschemaに対して更新操作を検証します。
    });

    res.status(200).json({
      status: 'success',
      data: {
        tour
      }
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err
    });
  }
};
```

### collectionで指定したIDのdocument削除

- async awaitでdata取得
- findOneAndDeleteメソッドの引数(削除したいidを指定)を指定
- try、catch文でエラーハンドリグ

```javascript
exports.deleteTour = async (req, res) => {
  try {
    await Tour.findOneAndDelete(req.params.id);

    res.status(204).json({
      status: 'success',
      data: null
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err
    });
  }
};
```
