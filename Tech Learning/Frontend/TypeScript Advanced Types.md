# TypeScript 高级类型技巧

## 条件类型
```typescript
type IsString<T> = T extends string ? true : false;
type A = IsString<"hello">; // true
type B = IsString<42>;      // false
```

## 模板字面量类型
```typescript
type EventName = `on${Capitalize<string>}`;
// "onClick" | "onChange" | ...
```

## infer 关键字
```typescript
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;
type UnpackPromise<T> = T extends Promise<infer U> ? U : T;
```

## 实用工具类型组合
```typescript
type DeepPartial<T> = {
  [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P];
};
```
