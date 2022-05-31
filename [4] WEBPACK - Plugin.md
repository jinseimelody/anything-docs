**[4] WEBPACK - Plugin**

Bản thân webpack được xây dựng trên `plugin system` tương tự như các plugin được sử dụng trong `webpack.config.js`

Plugin có thể thực hiện một số các công việc mà loader không thực hiện được

**I. Anatomy**

[tham khảo](https://webpack.js.org/concepts/plugins/#anatomy)

**II. Usage**

Trước khi plugin có thể nhận vào arguments/options, bạn cần tạo mới 1 instance cho plugin trong file `webpack.config.js`.

Tùy vào cách bạn sử dụng webpack, có nhiều cách để sử dụng plugin
- Configuration
- Node Api

**III. Configuration**

```
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack'); //to access built-in plugins
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    filename: 'my-first-webpack.bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        use: 'babel-loader',
      },
    ],
  },
  plugins: [
    new webpack.ProgressPlugin(),
    new HtmlWebpackPlugin({ template: './src/index.html' }),
  ],
};
```

Trong ví dụ trên `ProgressPlugin` được cấu hình để theo dõi quá trình build của webpack, và `HtmlWebpackPlugin` tạo file html bên trong đó include `my-first-webpack.bundle.js` thông qua **script** tag.


