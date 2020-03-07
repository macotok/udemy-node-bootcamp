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
- デフォルト値設定
- 一意を設定
- 前後の空白を埋める
- 配列の中に文字列が入る
- 時間の型定義と現在時刻のデフォルト値を設定

```javascript
const tourSchema = new mongoose.Schema({
  name: {
    type: String, // 型定義
    required: [true, 'A tour must have a name'], // 必須とエラーメッセージを設定
    unique: true, // 一意を設定
    trim: true  // 前後の空白を埋める('   hoge foo  ') -> ('hoge foo') 
  },
  rating: {
    type: Number,
    default: 4.5 // デフォルト値設定
  },
  price: Number, // 型のみ定義
  image: [String], // 配列の中に文字列が入る
  createdAt: {
    type: Date, // 時間の型定義
    default: Date.now() // 現在時刻をデフォルト値に設定
  }
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

## jsonファイルをschemaに則ってimport

- jsonファイルを読み込む

```javascript
const tours = JSON.parse(
  fs.readFileSync(`${__dirname}/tours-simple.json`, 'utf-8')
);
```

- `Tour`はmongooseで定義したschema
- `Tour`はmodelからimport
- `process.exit();`でアプリケーション終了

```javascript
const importData = async () => {
  try {
    await Tour.create(tours);
    console.log('Data successfully loaded');
  } catch (err) {
    console.log(err);
  }
  process.exit();
};
```

- `Tour`はmongooseで定義したschema
- `Tour`はmodelからimport
- `process.exit();`でアプリケーション終了

```javascript
const deleteData = async () => {
  try {
    await Tour.deleteMany();
    console.log('Data successfully deleted');
  } catch (err) {
    console.log(err);
  }
  process.exit();
};
```

## Filtering

### findメソッドとmongooseメソッドを使用

- findメソッドを使用
- `res.query`でquery取得

```javascript
const tours = await Tour.find({
  duration: 5,
  difficulty: 'easy'
});
```

- mongooseメソッドを使用
- `res.query`でquery取得

```javascript
const tours = await Tour.find()
  .where('duration')
  .equals(5)
  .where('difficulty')
  .equals('easy');
```

### filtering対象外を指定

```javascript
const queryObj = { ...req.query };
const excludedFields = ['page', 'sort', 'limit', 'fields'];
excludedFields.forEach(el => delete queryObj[el]);

Tour.find(queryObj);
```

### gte、gt、lte、ltに対応

- `/api/v1/tours?price[gte]=1500`に対応

```javascript
const queryObj = { ...req.query };
let queryStr = JSON.stringify(queryObj);
queryStr = queryStr.replace(/\b(gte|gt|lte|lt)\b/g, match => `$${match}`);
Tour.find(JSON.parse(queryStr));
```

### sortに対応

- `/api/v1/tours?sort=-price`に対応
- `-`を付けると昇順になる
- 複数指定する場合はカンマでつなげる`/api/v1/tours&sort=-price,-average`

```javascript
let query = Tour.find(JSON.parse(queryStr));
if (req.query.sort) {
  const sortBy = req.query.sort.split(',').join(' ');
  // { -price -average } 半角空けると複数sortに対応
  query = query.sort(sortBy);
} else {
  query = query.sort('-createdAt');
}
```

### Fieldを絞って抽出

- `/api/v1/tours?fields=name,price`に対応
- `-`を付けると抽出されない

```javascript
let query = Tour.find(JSON.parse(queryStr));
if (req.query.fields) {
  const fields = req.query.fields.split(',').join(' ');
  query = query.select(fields);
} else {
  query = query.select('-__v'); // `__v` fieldは抽出されない
}
```

### Pagination対応

- mongooseの`skip`メソッドを使用
- mongooseの`countDocuments`メソッドでdocument数取得
- document数以上のpaginationは作成されないようにハンドリング

```javascript
const page = req.query.page * 1 || 1;
const limit = req.query.limit * 1 || 100;
const skip = (page - 1) * limit;

query = query.skip(skip).limit(limit);

if (req.query.page) {
  const numTours = await Tour.countDocuments();
  if (skip >= numTours) throw new Error('this page does not exits');
}
```

## Aggregation(集計)

- mongoDBのメソッド`aggregate`でdataの集計を行う
- `$match`で該当のdataを集計
- `$group`で集計するグループを設定
- `$toUpper`で大文字に変換
- `$sum`で合計値を集計
- `$avg`で平均値を集計
- `$min`で最小値を取得
- `$max`で最大値を取得
- `$sort`で並び替え

```javascript
const stats = Tour.aggregate([
  {
    $match: { ratingsAverage: { $gte: 4.5 } }
  },
  {
    $group: {
      _id: { $toUpper: '$difficulty' },
      numTours: { $sum: 1 },
      numRatings: { $sum: '$ratingsQuantity'},
      avgRating: { $avg: '$ratingsAverage' },
      avgPrice: { $avg: '$price' },
      minPrice: { $min: '$price' },
      maxPrice: { $max: '$price' }
    }
  },
  {
    $sort: { avgPrice: 1 }
  }
]);
```

集計結果
```json
"stats": [
    {
        "_id": "DIFFICULT",
        "numTours": 2,
        "numRatings": 41,
        "avgRating": 4.7,
        "avgPrice": 748.5,
        "minPrice": 500,
        "maxPrice": 997
    },
    {
        "_id": "MEDIUM",
        "numTours": 3,
        "numRatings": 70,
        "avgRating": 4.8,
        "avgPrice": 1663.6666666666667,
        "minPrice": 497,
        "maxPrice": 2997
    }
]
```

- `$unwind`で対象の配列のdocumentを展開
- 日時を抽出するときの書き方`new Date(${year}-01-01)`
- `$month`で1-12で表示
- `$push`で`$sum`の合計値が複数の場合に配列でList化
- `$addFields`で集計のFieldを追加
- `$project`で集計する項目を指定
- `$limit`で集計した表示数を指定

```javascript
const year = req.params.year * 1;

const plan = await Tour.aggregate([
  {
    $unwind: '$startDates'
  },
  {
    $match: {
      startDates: {
        $gte: new Date(`${year}-01-01`),
        $lte: new Date(`${year}-12-31`)
      }
    }
  },
  {
    $group: {
      _id: { $month: '$startDates' },
      numTourStats: { $sum: 1 },
      tours: { $push: '$name' }
    }
  },
  {
    $addFields: { month: '$_id' }
  },
  {
    $project: {
      _id: 0
    }
  },
  {
    $sort: {
      numTourStats: -1
    }
  },
  {
    $limit: 12
  }
]);
```

集計結果

```json
"plan": [
    {
        "numTourStats": 3,
        "tours": [
            "The Forest Hiker",
            "The Sea Explorer",
            "The Sports Lover"
        ],
        "month": 7
    },
    {
        "numTourStats": 2,
        "tours": [
            "The Sea Explorer",
            "The Park Camper"
        ],
        "month": 8
    },
    {
        "numTourStats": 2,
        "tours": [
            "The Forest Hiker",
            "The Star Gazer"
        ],
        "month": 10
    },
    {
        "numTourStats": 2,
        "tours": [
            "The Sea Explorer",
            "The City Wanderer"
        ],
        "month": 6
    },
    {
        "numTourStats": 2,
        "tours": [
            "The Forest Hiker",
            "The Wine Taster"
        ],
        "month": 4
    },
    {
        "numTourStats": 2,
        "tours": [
            "The Wine Taster",
            "The Sports Lover"
        ],
        "month": 9
    },
    {
        "numTourStats": 2,
        "tours": [
            "The City Wanderer",
            "The Star Gazer"
        ],
        "month": 3
    },
    {
        "numTourStats": 1,
        "tours": [
            "The City Wanderer"
        ],
        "month": 5
    },
    {
        "numTourStats": 1,
        "tours": [
            "The Northern Lights"
        ],
        "month": 12
    },
    {
        "numTourStats": 1,
        "tours": [
            "The Wine Taster"
        ],
        "month": 2
    }
]
```
