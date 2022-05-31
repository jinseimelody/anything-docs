**[3] WEBPACK - Loaders**

Loaders (bộ chuyển đổi) có thể chuyển đổi file từ ngôn ngữ này sang ngôn ngữ khác như từ ts -> js..., nó thậm chí còn cho phép import css files một cách trực tiếp từ JS module.

**I. Example**

Sử dụng loaders để load một file css hoặc convert ts -> js. Để làm điều đó, đầu tiên bạn cần tải và cài đặt loader thích hợp.

```
npm install --save-dev css-loader ts-loader
```
Và sau đó cấu hình webpack để sử dụng `css-loader` cho mọi file `.css` và `ts-loader` cho mọi file `.ts`.
```
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' },
      { test: /\.ts$/, use: 'ts-loader' },
    ],
  },
};

```

**II. Using loaders**

Có 2 cách để sử dụng loaders trong ứng đụng
1. Configuration (khuyên dùng): cấu hình trong file `webpack.config.js`
2. Inline: chỉ định trực tiếp mỗi khi import

`WAR:` loaders có thể được sử dụng trong webpack 4 CLI nhưng trong tương lai sẽ không còn được dùng trong webpack 5

**III. Configuration**

module.rules cho phép cấu hình một vài loaders trong webpack.

Loader được đánh giá/ thực thi từ phải sang trái boặc từ dưới lên trên. Trong ví dụ bên dưới loader đầu tiên chạy là `sass-loader` tiếp theo là `css-loader` và cuối cùng là `style-loader`.

```
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          // [style-loader](/loaders/style-loader)
          { loader: 'style-loader' },
          // [css-loader](/loaders/css-loader)
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          },
          // [sass-loader](/loaders/sass-loader)
          { loader: 'sass-loader' }
        ]
      }
    ]
  }
};
```

**IV. Inline**

Vì cách này không được khuyên dùng nên nếu bạn muốn tìm hiểu chi tiết hãy [tham khảo](https://webpack.js.org/concepts/loaders/#inline)

**V. Loader feature**

[tham khảo](https://webpack.js.org/concepts/loaders/#loader-features)


**VI. Resolving loaders**

[tham khảo](https://webpack.js.org/concepts/loaders/#resolving-loaders)



