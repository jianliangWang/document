# Buffer的使用

字符串的长度和buffer的长度没有任何关系

## Buffer长度

```js
const buffer = Buffer.alloc(128);

const length = buffer.write("helloworld", "utf-8");

console.log(length);
```

10

> 如果有中文会是什么结果？

```js
const buffer = Buffer.alloc(128);

const length = buffer.write("helloworld你好", "utf-8");

console.log(length);
```

16 

输出结果变成了16。中文一个汉子占用3个字节

## Buffer长度比较

```js
const hello = Buffer.from('hello');
const world = Buffer.from('world');

console.log(hello.compare(world));
```

结果：-1

## Buffer转字符串

```js
const buffer = Buffer.alloc(3);

buffer[0] = 65;
buffer[1] = 66;
buffer[2] = 67;

console.log(buffer.toString("utf-8"));
```

## 字符串转Buffer

```js
const str = "abcde大";

const buffer = Buffer.from(str);
console.log(str.length);
console.log(buffer.length);
```

## Buffer拼接

```js
const buffer1 = Buffer.from("hello");
const buffer2 = Buffer.from("world");
const buffer3 = Buffer.from("welcome");

const bufferArray=[buffer1, buffer2, buffer3];

const bufferResult = Buffer.concat(bufferArray);

console.log(bufferResult.length);

console.log(bufferResult.toString());
```

## Buffer 与 Json转换

```js
const buffer = Buffer.from('你好世界');
const jsonString = JSON.stringify(buffer);
console.log(jsonString);

const jsonObject = JSON.parse(jsonString);

console.log(jsonObject);

const bufferString = Buffer.from(jsonObject);
console.log(bufferString);
```

## 检测编码是否有效

```js
const str = "utf8";
const str1 = "utf-8";
const str2 = "UTF-8";
const str3 = "GBK";
const str4 = "gb2312";

console.log(Buffer.isEncoding(str));
console.log(Buffer.isEncoding(str1));
console.log(Buffer.isEncoding(str2));
console.log(Buffer.isEncoding(str3));
console.log(Buffer.isEncoding(str4));
```

## 类型检查，是否是Buffer类型

```js
const buffer = Buffer.from("hello");


console.log(Buffer.isBuffer(buffer));
console.log(Buffer.isBuffer(myObj));
```
