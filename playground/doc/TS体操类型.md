![pexels-axp-photography-500641970-18717344](http://assest.sablogs.cn/img/typora/pexels-axp-photography-500641970-18717344.jpg)

# TS体操

> 题目链接如下
>
> [type-challenges/README.zh-CN.md at main · type-challenges/type-challenges (github.com)](https://github.com/type-challenges/type-challenges/blob/main/README.zh-CN.md)

## 简单

### 4. 实现pick

```ts
type MyPick<T, K extends keyof T> = {
  [P in K]: T[P]
}
```

### 7. 对象属性只读

```ts
type MyReadonly<T> = {
 readonly [P in keyof T]: T[P]
}
```

### 11. 元组转换为对象

- T[number] 可以去遍历联合类型

```ts
type TupleToObject<T extends readonly PropertyKey[]> = {
  [P in T[number]]: P
}
```

### 14. 第一个元素

```ts
type First<T extends any[]> = T extends [] ? never : T[0]
```

### 18. 获取元组长度

- 利用类数组元素的`length`属性

```ts
type Length<T extends readonly PropertyKey[]> = T['length']
```

### 43. 实现 Exclude

```ts
// T中的某一项如果在U中(T extends U)，返回never移除U，否则返回T。
type MyExclude<T, U> = T extends U ? never : T;
```

### 189. Awaited

```ts
// 思路：
// 1. infer U - 反向获取到泛型的入参类型
// 2. 对于嵌套Promise的情况做递归调用处理，通过U extends PromiseLike<any>，判断是否为Promise
type MyAwaited<T extends PromiseLike<any>> = T extends PromiseLike<infer U> ? U extends PromiseLike<any> ? MyAwaited<U> : U : never
```

**infer：**

infer作用：**推导泛型参数**

```ts
// infer 例子
type numberPromise = Promise<number>;
type n = numberPromise extends Promise<infer P> ? P : never; // number
```

### 268. If

```ts
type If<C extends boolean, T, F> = C extends true ? T : F
```

### 533. Concat

```ts
type Tuple = readonly unknown[];
type Concat<T extends Tuple, U extends Tuple> = [...T, ...U]
```

### 898. Includes

这题很难，做个标记

主要是通过`T extends [infer First, ...infer Rest]`，每次校验`arr`的第一位，看是否等于U，以此递归

```ts
type Includes<T extends readonly unknown[], U> =
  T extends [infer First, ...infer Rest]
    ? Equal<First, U> extends true ? true : Includes<Rest, U>
    : false;
```

### 3057. Push

```ts
type Push<T extends unknown[], U> = [...T, U]
```

### 3060. Unshift

```ts
type Unshift<T extends unknown[], U> = [U, ...T];
```

###  3312. Parameters

这个重点是掌握参数结构，也是通过infer去获取入参的类型

```ts
type MyParameters<T extends (...args: any[]) => any> =  T extends (...args: infer S) => void ? S : any
```

## 中等

### 2. 获取函数返回类型

```ts
type MyReturnType<T extends Function> = T extends (...args: any) =>  infer R ? R : never
```

###  3. 实现 Omit



## 知识点

### 协变和逆变

- 协变：类型推导到其子类型的过程，A | B -> A & B 就是一个协变

- 逆变：类型推导到其超类型的过程

## 参考

- [type-challenges/type-challenges: Collection of TypeScript type challenges with online judge (github.com)](https://github.com/type-challenges/type-challenges?tab=readme-ov-file)
- [TypeScript：一文搞懂 infer - 掘金 (juejin.cn)](https://juejin.cn/post/6998347146709696519)

