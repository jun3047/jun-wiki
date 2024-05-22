# 구속된 JS = TypeScript



### interface



### type



### Generic



### Generic Util

* **Partial\<T>**: 모든 속성을 선택적으로 만듭니다.

```typescript
interface Person {
  name: string;
  age: number;
}
const partialPerson: Partial<Person> = { name: "Alice" };
```



* **Readonly\<T>**: 모든 속성을 읽기 전용으로 만듭니다.

```typescript
const readonlyPerson: Readonly<Person> = { name: "Alice", age: 25 };
// readonlyPerson.age = 30; // Error
```



* **Omit\<T, 제외할 타입>**: 특정

```typescript
const readonlyPerson: Readonly<Person> = { name: "Alice", age: 25 };
// readonlyPerson.age = 30; // Error
```



* **Pick\<T, 선택할 타입>**: \




🧐 이것만 알면 됨 (interface, type)
