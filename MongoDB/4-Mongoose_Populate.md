### Mongoose

```js
import express from 'express';
import mongoose from 'mongoose';

const app = express();
app.use(express.json());

async function start() {
  try {
    await mongoose.connect(process.env.MONGO_URI);

    console.log('MongoDB connected');

    app.listen(8080, () => console.log('server runing'));
  } catch (err) {
    console.log('error to connect DB', err);
  }
}

start();
```

به تو اجازه می‌دهد Schema تعریف کنی.

حالا اگر کسی داده اشتباه بفرستد، Mongoose قبل از ذخیره شدن آن را بررسی می‌کند.

اینجا ساختار Document را مشخص کرده‌ای.

```js
import { Schema, model } from 'mongoose';
import bcrypt from 'bcrypt';

const userSchema = new Schema({
  email: {
    type: String,
    required: true,
    unique: true,
  },

  password: {
    type: String,
    required: true,
  },

  name: {
    type: String,
    required: true,
  },

  age: {
    type: Number,
    min: 18,
  },

  role: {
    type: String,
    default: 'user',
    enum: ['user', 'admin'],
  },

  posts: [
    {
      type: Schema.Types.ObjectId,
      ref: 'Post',
    },
  ],
});


userSchema.methods.comparePassword = async function (candidatePassword) {
  return await bcrypt.compare(candidatePassword, this.password);
};

userSchema.statics.findByEmail = function (email) {
  return this.findOne({ email });
};

// methods & statics
class User {
  isAdmin() { ... }           // instance method → methods
  static findByEmail() { ... } // static method → statics
}

export default model('User', userSchema);
```

```js
import { Schema, model } from 'mongoose';

const postSchema = new Schema(
  {
    title: {
      type: String,
      required: true,
    },

    content: {
      type: String,
      required: true,
    },

    creator: {
      type: Schema.Types.ObjectId,
      ref: 'User',
      required: true,
    },
  },
  { timestamps: true }

  /*{ خودش اینارو اضافه میکنه
    "createdAt": "...",
    "updatedAt": "..."
  }*/
);

export default model('Post', postSchema);
```

```js
await User.create({
  name: 'Amin',
  age: 27,
  email: 'amin@gmail.com',
});

const users = await User.find();

const user = await User.findOne({
  email: 'amin@gmail.com',
});

const user = await User.findById(id);

await User.updateOne({ email: 'amin@gmail.com' }, { age: 28 });

await User.deleteOne({
  email: 'amin@gmail.com',
});
```

### Populate

قابلیتی در Mongoose است که فیلدهای Reference (ObjectId) را با اطلاعات واقعی سند مرتبط جایگزین می‌کند. این قابلیت از نظر مفهوم شبیه JOIN در SQL است، هرچند پیاده‌سازی آن متفاوت است.

اگر قبلاً با SQL کار کرده باشی، احتمالاً JOIN را می‌شناسی.

```sql
SELECT *
FROM posts
JOIN users
ON posts.userId = users.id;
```

در MongoDB چیزی به نام JOIN وجود ندارد، اما Mongoose قابلیتی به نام populate() دارد که کار مشابهی انجام می‌دهد.

```js
// User
const userSchema = new mongoose.Schema({
  name: String,

  email: String,

  password: String,
});

// Post
const postSchema = new mongoose.Schema({
  title: String,

  user: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
  },
});
```

بدون Populate

```js
const post = await Post.findOne();
```

فقط شناسه کاربر را داری.

```json
{
  "_id": "...",
  "title": "Learning MongoDB",
  "user": "687f5ab8..."
}
```

با Populate

اینجوری همه اطلاعات یوزر حتی پسوورد هم میاد

```js
const post = await Post.findOne().populate('user');
```

```json
{
  "_id": "...",
  "title": "Learning MongoDB",

  "user": {
    "_id": "...",
    "name": "Amin",
    "email": "amin@gmail.com",
    "password": "1234"
  }
}
```

```js
const post = await Post.findOne().populate('user', 'name email');
```

```json
{
  "_id": "...",
  "title": "Learning MongoDB",

  "user": {
    "_id": "...",
    "name": "Amin",
    "email": "amin@gmail.com"
  }
}
```

فرض کن Post هم نویسنده دارد و هم دسته‌بندی:

هر دو Reference پر می‌شوند

```js
const post = await Post.find().populate('user').populate('category');
```
