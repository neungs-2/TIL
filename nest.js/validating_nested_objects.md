# **Validating Nested Objects** with class-validator
- `@ValidateNested()`와 `@Type()` 데코레이터를 사용

```ts
import { ValidateNested } from 'class-validator';
import { Type } from 'class-transformer';

// Nested Object를 Object가 감싸고 있을 때
export class Post {
  @ValidateNested()
  @Type(() => User)
  user: User;
}

// Nested Object를 Array가 감싸고 있을 때
export class User {
  @ValidateNested({ each: true })
  @Type(() => Post)
  posts: Post[];
}
```
