### اتصال MongoDB به Node.js

معمولاً این کار را نمی‌کنیم که در هر فایل یک اتصال جدید بسازیم.

به جای آن یک فایل مثل database.js یا db.ts داریم:

```js
// db.js

import { MongoClient } from 'mongodb';
let db;

export async function connectDB() {
  const client = new MongoClient(process.env.MONGO_URI);
  await client.connect();

  db = client.db('shop');

  console.log('MongoDB connected');
}

export function getDB() {
  if (!db) throw new Error('DB not initialized');
  return db;
}
```

```js
// server.js
import express from 'express';
import { connectDB } from './db.js';

const app = express();
app.use(express.json());

async function start() {
  try {
    await connectDB();
    app.listen(3000, () => console.log('Server running'));
  } catch (err) {
    console.log('Failed to start server:', err);
    process.exit(1);
  }
}

start();
```

انتخاب Collection

```js
const users = db.collection('users');
```

```js
await users.insertOne({
  name: 'Amin',
  age: 27,
});

const username = await users.findOne({
  name: 'Amin',
});

const result = await users.find().toArray();

await users.deleteOne({
  name: 'Amin',
});
```

یه crud کامل

```js
import { getDb } from '../util/database.js';
import mongodb from 'mongodb';

export class User {
  constructor(name, email, cart, id) {
    this.name = name;
    this.email = email;
    this.cart = cart;
    this._id = id;
  }

  async save() {
    const db = getDb();

    if (this._id) {
      const { _id, ...rest } = this;
      return await db.collection('users').updateOne({ _id: new mongodb.ObjectId(this._id) }, { $set: rest });
    }

    return await db.collection('users').insertOne(this);
  }

  async addToCart(product) {
    const db = getDb();

    const result = await db.collection('users').updateOne(
      {
        _id: new mongodb.ObjectId(this._id),
        'cart.items.productId': product._id,
      },
      { $inc: { 'cart.items.$.quantity': 1 } }
    );

    if (result.modifiedCount === 0) {
      return db.collection('users').updateOne(
        {
          _id: new mongodb.ObjectId(this._id),
        },
        { $push: { 'cart.items': { productId: product._id, quantity: 1 } } }
      );
    }

    return result;
  }

  async deleteItemFromCart(productId) {
    const db = getDb();

    return await db.collection('users').updateOne(
      {
        _id: new mongodb.ObjectId(this._id),
      },
      { $pull: { 'cart.items': { productId: new mongodb.ObjectId(productId) } } }
    );
  }

  async getCart() {
    const db = getDb();

    const productIds = this.cart.items.map((item) => item.productId);

    const products = await db
      .collection('products')
      .find({ _id: { $in: productIds } })
      .toArray();

    return products.map((product) => {
      const cartItem = this.cart.items.find((item) => item.productId.toString() === product._id.toString());

      return { ...product, quantity: cartItem.quantity };
    });
  }

  async addOrder() {
    const db = getDb();

    const products = await this.getCart();

    await db.collection('orders').insertOne({
      user: { _id: this._id, name: this.name },
      products,
    });

    await db.collection('users').updateOne(
      {
        _id: new mongodb.ObjectId(this._id),
      },
      { $set: { cart: { items: [] } } }
    );

    this.cart.items = [];
  }

  async getOrders() {
    const db = getDb();

    return await db
      .collection('orders')
      .find({ 'user._id': new mongodb.ObjectId(this._id) })
      .toArray();
  }

  static async findById(userId) {
    const db = getDb();

    const user = await db.collection('users').findOne({ _id: new mongodb.ObjectId(userId) });

    return new User(user.name, user.email, user.cart, user._id);
  }
}
```
