**[2] WEBPACK - Output**

output configuration options nói cho webpack biết cách mà bạn muốn compiled file được tạo ra. Lưu ý, trong khi có thể cấu hình nhiều entry points thì chỉ có thể cấu hình một output được chỉ định.

**I. Usage**

Để sử dụng tối thiểu bạn cần cung cấp output.filename trong `webpack.config.js`
```
module.exports = {
  output: {
    filename: 'bundle.js',
  },
};

```
Cấu hình như trên cho phép tạo ra 1 file bundle duy nhất vào trong thư mục "dist" nằm cùng cấp với file `webpack.config.js`

**II. Multiple Entry Points**

Nếu configuration file tạo ra nhiều "chunk" (giống như khi chỉ định nhiều entry point hoặc sử dụng plugin như CommonsChunkPlugin), bạn nên sử dụng "subsitutions" để chắc chắn rằng mỗi file sở hữu một tên duy nhất.

```
module.exports = {
  entry: {
    app: './src/app.js',
    search: './src/search.js',
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist',
  },
};

// writes to disk: ./dist/app.js, ./dist/search.js
```

**III. Advanced**

Bên dưới là ví dụ cho việc sử dụng CDN và hashes cho assets.

```
module.exports = {
  //...
  output: {
    path: '/home/proj/cdn/assets/[fullhash]',
    publicPath: 'https://cdn.example.com/assets/[fullhash]/',
  },
};

```
Trong trường hợp publicPath không được chỉ định trong output object, nó có thể được chỉ định thông qua biến __webpack_public_path__ trong `webpack.config.js`
```
__webpack_public_path__ = myRuntimePublicPath;

// rest of your application entry
```