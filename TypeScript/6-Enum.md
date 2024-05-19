# TypeScript - Enum Type

An `enum` (short for "enumeration") is a way to define a set of named constants. TypeScript provides both numeric and string-based enums.

## Use Cases

Enums are useful when you need a set of named constants that represent a collection of related values, such as directions, states, or statuses.

Enums help in making code more readable and maintainable by providing meaningful names to constants instead of using magic numbers or strings.

## Numeric Enums

Numeric enums are the default. They assign a numeric value to each member, starting from `0` by default.

Example:

```typescript
enum Direction {
  Up,
  Down,
  Left,
  Right,
}

let move: Direction = Direction.Up;
console.log(move); // Output: 0
```

## String Enums

String enums allow you to explicitly define the string value for each member.

```typescript
enum Direction {
  Up = 'UP',
  Down = 'DOWN',
  Left = 'LEFT',
  Right = 'RIGHT',
}

let move: Direction = Direction.Up;
console.log(move); // Output: "UP"
```

## Heterogeneous Enums

Heterogeneous enums allow both string and numeric values in the same enum. This is less common and typically discouraged.

```typescript
enum MixedEnum {
  No = 0,
  Yes = 'YES',
}

let answer: MixedEnum = MixedEnum.Yes;
console.log(answer); // Output: "YES"
```

## Enum Member Values

You can specify the value for enum members explicitly, and subsequent members will auto-increment from that value.

```typescript
enum Status {
  New = 1,
  InProgress,
  Done,
}

console.log(Status.New); // Output: 1
console.log(Status.InProgress); // Output: 2
console.log(Status.Done); // Output: 3
```

## Reverse Mapping

Numeric enums support reverse mapping, where you can get the name of the enum member from its value.

```typescript
enum Direction {
  Up,
  Down,
  Left,
  Right,
}

let directionName: string = Direction[0];
console.log(directionName); // Output: "Up"
```
