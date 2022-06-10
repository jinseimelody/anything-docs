# ES - ECMAScript
ECMAScript do hiệp hội các nhà sản xuất máy tính Châu Âu đề xuất làm tiêu chuẩn của ngôn ngữ Javascript trên các browser

## ES6
Hay còn được gọi là ES2015 mang đến nhiều tính năng mới học hỏi các ngôn ngữ lập trình cấp cao khác

## Các chức năng mới trong ES6
### 1. Arrow function
Bạn có thể tạo ra hàm bằng cách sử dụng dấu mũi tên `=>` tương tự nhu delegete function trong C#

### 2. Block scoped
Block Scoped là phạm vi trong một khối, nghĩa là chỉ hoạt động trong phạm vi được khai báo bời cặp {}.
- Ở Es6 người ta sử dụng biến `let` để khai báo cho biến trong cặp {}
- Let tạo ra một biến chỉ có thể truy cập được trong block bao quanh nó, khác với var - tạo ra một biến có phạm vi truy cập xuyên suốt function chứa nó.
- Biến `const` : dùng để khai báo một hằng số - là một giá trị không thay đổi được trong suốt quá trình chạy.

### 3. Destructuring Assignments
Bạn có thể khởi tạo các biến từ một mảng hoặc object bằng một dòng code đơn giản.

```
const fruilds = ['orange', 'mango', 'apple']
const [foo, bar, far] = fruilds

// hoặc
const [foo, ...rest] = fruilds

=============================================

const person = {
    name: 'john doe',
    age: 17,
    gender: 'male'
}

const { name, age, gender } = person

// hoặc
const {age , ...orther} = person
```

Bạn có thể merge array hoặc object bằng một dòng code đơn giản.
```
const fruilds = ['orange', 'mango', 'apple']
const names = ['john', 'jane']

const merged = [...fruilds, ...names]
// =>  ['orange', 'mango', 'apple', 'john', 'jane']

=============================================

const john = {
    name: 'john doe',
    age: 17,
    gender: 'male'
}

const jane = {
    name: 'jane doe',
    humble: true
}

const merged = {...john, ...jane, name: `peter tèo`}
// =>  {name: 'john doe', age: 17, gender: 'male', humble: true}
```

### 4 Default Parameters
Bạn có thể gán giá trị mặc định cho các tham số
```
const logging = (message = 'Hello you guys!') => {
    console.log(message)
}

logging('Hello world');
// => 'Hello world'

logging();
// => 'Hello you guys!'
```

### Template String
```
const name = 'John Weak'

ES5
console.log('Hello' + name)

ES6
console.log(`Hello ${name}`)
```

### Các kiểu dữ liệu phức tạp mới được thêm vào
Set: tương tự nhu Dictionary trong C#

[xem thêm](https://www.w3schools.com/js/js_es6.asp)