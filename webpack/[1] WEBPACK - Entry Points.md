**[1] WEBPACK - Entry Points**

Như đã đề cập trong phần mở đầu, có nhiều cách để khai báo một entry trong webpack.configuration.js. Note này sẽ hướng dẫn những cách đó bên cạnh đó sẽ giải thích trong trường hợp nào thì sử dụng cái gì

**I. Single entry (Shorthand) syntax**
```
module.exports = {
  entry: './path/to/my/entry/file.js',
};

```
tương đương với

```
module.exports = {
  entry: {
    main: './path/to/my/entry/file.js',
  },
};
```

Bạn cũng có thể truyền vào array of file paths để chỉ ra rằng bạn muốn sử dụng `multiple-main entry`. 
```
module.exports = {
  entry: ['./src/file_1.js', './src/file_2.js'],
  output: {
    filename: 'bundle.js',
  },
};
```
Syntax này sẽ được sử dụng trong trường hợp bạn muốn inject nhiều file có liên quan với nhau   và gom nó lại trong 1 dependency-graph

**II. Object syntax**
```
module.exports = {
  entry: {
    app: './src/app.js',
    adminApp: './src/adminApp.js',
  },
};

```
Object syntax khá dài dòng (**verbose**), Tuy nhiên viết như thế này sẽ dễ mở rộng hơn kiểu shorthand

`EntryDescription` object's properties:
1. dependOn: chỉ định những entry point mà entry point hiện tại phụ thuộc vào, những entry point này sẽ được load trước khi load entry point hiện tại.
2. filename: chỉ định tên của mỗi output file.
3. import: chỉ định các module được tải khi khởi động quá trình bundle.
4. library: chỉ định `library options` để bundle một library từ entry hiện tại.
5. runtime: chỉ định runtime, khi được chỉ định new runtime chunk sẽ được tạo ra, có thể set thành `false` để bỏ qua việc tạo mới runtime chunk kể từ phiên bản 5.43.0.
6. publicPath: chỉ định một public URL cho output file thứ mà có thể dùng để truy cập file đó từ browser.

```
module.exports = {
  entry: {
    a2: 'dependingfile.js',
    b2: {
      dependOn: 'a2',
      import: './src/app.js',
    },
  },
};
```
`WAR: import` và `dependOn` không nên được sử dụng cùng nhau trong một entry, vì vậy cấu hình sau sẽ không hợp lệ và gây ra lỗi
```
module.exports = {
  entry: {
    a2: './a',
    b2: {
      runtime: 'x2',
      dependOn: 'a2',
      import: './b',
    },
  },
};
```
`WAR:` hãy chắc rằng `runtime` không được trỏ tới tên entry point đã có sẵn. vì vậy cấu hình sau sẽ không hợp lệ và gây ra lỗi
```
module.exports = {
  entry: {
    a1: './a',
    b1: {
      runtime: 'a1',
      import: './b',
    },
  },
};
```
`WAR:` `dependOn` không được tạo thành vòng lặp, vì vậy cấu hình sau sẽ không hợp lệ và gây ra lỗi
```
module.exports = {
  entry: {
    a3: {
      import: './a',
      dependOn: 'b3',
    },
    b3: {
      import: './b',
      dependOn: 'a3',
    },
  },
};

```

**III. Scenarios**

Trong thự tế entry có thể được cấu hình theo các case sau:

**1. Separate App and Vendor Entries**

`webpack.config.js`
```
module.exports = {
  entry: {
    main: './src/app.js',
    vendor: './src/vendor.js',
  },
};
```

`webpack.prod.js`
```
module.exports = {
  output: {
    filename: '[name].[contenthash].bundle.js',
  },
};
```

`webpack.dev.js`
```
module.exports = {
  output: {
    filename: '[name].bundle.js',
  },
};
```

**What does this do?** Bằng cách config như ví dụ bên trên bạn nói cho webpack biết mình cần 2 separate entry points.

**Why?** khi import những thư viện hoặc file không bị sửa đổi (e.g. Bootstrap, jQuery, images, etc..) vào vendor.js, contenthash cho phép trình duyệt có thể cache chúng một cách riêng biệt và giảm thời gian tải.

**2. Multi-Page Application**

`webpack.config.js`
```
module.exports = {
  entry: {
    pageOne: './src/pageOne/index.js',
    pageTwo: './src/pageTwo/index.js',
    pageThree: './src/pageThree/index.js',
  },
};
```

**What does this do?** Bằng cách config như ví dụ bên trên bạn nói cho webpack biết mình cần 3 separate dependency graphs => 1 bundle duy nhất. vì vậy khi sử dụng cách này cần optimize bằng cách sử dụng `optimization.splitChunks` tham khảo *[webpack_cache](https://hocwebchuan.com/tutorial/webpack/webpack_cache.php)*

