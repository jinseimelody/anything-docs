**[0] WEBPACK - Concepts**

Webpack là một static module bundler cho các ứng dụng viết bằng JS. Khi webpack xử lý đóng gói nó sẽ tự động tạo một dependency graph từ một hoặc nhiều entry points và kết nối những module cần thiết lại với nhau tạo thành một hoặc nhiều file bundle

Từ version 4.0.0, webpack không còn yêu cầu file config để bundle một project nữa, tuy nhiên (Nevertheless) nó vẫn có thể cấu hình để phù hợp với yêu cầu sử dụng.

Để bắt đầu bạn chỉ cần hiểu những bước xử lý cơ bản (core concept) sau.

1. Entry

2. Output

3. Loader

4. Plugins

5. Mode

6. Brower Compatibility

Tài liệu này được dự định (intended) dùng để cung cấp cái nhìn tổng quát (high-level overview) về các khái niệm nêu trên.

Để hiểu rõ hơn ý tưởng đằng sau module bundlers và cách nó thực sự hoạt động (under the hood), tham khảo (consult) những tài liệu sau

- https://www.youtube.com/watch?v=UNMkLHzofQI

- https://www.youtube.com/watch?v=Gc9-7PBqOC8

- https://github.com/ronami/minipack

**I. Entry**

Một entry point chỉ ra (indicates) module nào nên được sử dụng để tạo ra dependency graph tạm gọi là root. webpack sẽ tìm ra những module và thư viện mà root cần sử dụng


Mặc định "./src/index.js", nhưng bạn có thể chỉ định entry là một file khác bằng cách sử dụng setting sau trong webpack.config.js

```
module.exports = {

  entry: "./path/to/my/entry/file.js"

}
```

**II. Output**

Thuộc tính output nói cho webpack biết bạn muốn tạo ra file bundles ở đâu và đặt tên nó như thế nào. Mặc định output tại folder "./dist" với tên "./dist/main.js", nhưng bạn có thể cấu hình nó bằng cách sử dụng setting sau trong webpack.config.js


```
module.exports = {

  output: {

    path: path.resolve(__dirname, 'dist'),

    filename: 'my-first-webpack.bundle.js',

  },

};

```

**III. Loaders**

(Out of the box), webpack chỉ có thể hiểu đc JS và JSON files. Loaders là thứ cho phép webpack xử lý các dạng file khác và chuyển đổi chúng thành các module hợp lệ thứ mà được sử dụng (consumed) trong ứng dụng của bạn và thêm nó vào dependency graph

Ở high-level, loader có 2 thuộc tính trong webpack.configuration.js

```
module.exports = {

  module: {

    rules: [{ test: /\.txt$/, use: 'raw-loader' }],

  },

};
```

1. thuộc tính "test" xác định những file nào sẽ được chuyển đổi

2. thuộc tính "use" xác định loader nào sẽ được dùng để xử lý việc chuyển đổi niêu trên

**IV. Plugins**

Trong khi loader được sử dụng để chuyển đổi một số loại module nhất định, plugins có thể được tận dụng (leveraged to perform) để thực hiện nhiều loại nhiệm vụ hơn, ví dụ như tối ưu hóa việc đóng gói, quản lý asset và inject các biến môi trường

Để sử dụng một plugin, bạn cần phải require() nó và thêm nó vào plugins array trong file webpack.configuration.js. Hẩu hết các plugins có thể được tùy chỉnh thông qua options. Vì bạn có thể sử dụng plugin nhiều lần trong config cho nhiều mục đích khác nhau, bạn cần phải tạo ra một instance của plugin đó bằng cách gọi nó thông qua phương thức "new"

```
module.exports = {

  plugins: [new HtmlWebpackPlugin({ template: './src/index.html' })],

};
```

Trong ví dụ bên trên, html-webpack-plugin tạo ra một file html trong ứng dụng của bạn và tự động inject tất cả bundle bạn tạo ra vào trong file html này.

**V. Mode**

Bằng cách setting mode "development", "production" hoặc "none" trong file webpack.configuration.js, bạn có thể bật tắt "webpack's built-in optimizations" thứ sẻ tối ưu hóa code trong bundle để phù hợp với môi trường mà bạn chỉ định, giá trị mặc định sẽ được cài đặt là "production".

```
module.exports = {

  mode: 'production',

};
```

**VI. Browser Compatibility**

Webpack hỗ trợ tất cả các trình duyệt miễn là nó có hỗ trợ ES5 (IE 8 và những phiên bản thấp hơn sẽ không được hỗ trợ). Webpack cần Promise for import() và require.ensure(). Nếu bạn muốn hỗ trợ những trình duyệt cũ hơn bạn cần load một polyfill trước khi sử dụng những hàm trên

  

**VII. Environment**

Webpack 5 chạy trên Node.js version 10.13.0+.