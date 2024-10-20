# TypeORM Relations

In the context of NestJS and TypeORM, TypeORM supports several types of relations between entities, allowing you to model real-world relationships within your application. The most common types of relations are:

1. **One-to-One** (`@OneToOne`)
2. **One-to-Many** (`@OneToMany`)
3. **Many-to-One** (`@ManyToOne`)
4. **Many-to-Many** (`@ManyToMany`)

Here’s how each type of relation can be defined within a NestJS application using TypeORM, along with examples:

---

### 1. **One-to-One Relation** (`@OneToOne`)

A `One-to-One` relationship exists when one entity is related to one other entity, and vice versa. For instance, a `User` entity may have one `Profile`.

**Example**:
Let's create two entities, `User` and `Profile`, where each user has one profile.

```typescript
import {
  Entity,
  PrimaryGeneratedColumn,
  Column,
  OneToOne,
  JoinColumn,
} from "typeorm";

@Entity()
export class Profile {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  bio: string;

  @OneToOne(() => User, (user) => user.profile)
  @JoinColumn()
  user: User;
}

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  username: string;

  @OneToOne(() => Profile, (profile) => profile.user)
  profile: Profile;
}
```

- `@OneToOne` defines the relationship.
- `@JoinColumn` is used to specify which side will own the relation and hold the foreign key. You need it on one side of the relation.

### 2. **One-to-Many / Many-to-One Relation** (`@OneToMany`, `@ManyToOne`)

A `One-to-Many` and `Many-to-One` relationship exist when one entity is related to many others, but the other entities are related to only one. For example, a `User` can have many `Posts`, but each `Post` is associated with one `User`.

**Example**:
Let’s create a `User` entity that has many `Posts`.

```typescript
import {
  Entity,
  PrimaryGeneratedColumn,
  Column,
  ManyToOne,
  OneToMany,
} from "typeorm";

@Entity()
export class Post {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  title: string;

  @ManyToOne(() => User, (user) => user.posts)
  user: User;
}

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  username: string;

  @OneToMany(() => Post, (post) => post.user)
  posts: Post[];
}
```

- `@ManyToOne` is defined on the `Post` side (the "many" side), indicating that each post belongs to one user.
- `@OneToMany` is defined on the `User` side (the "one" side), indicating that the user has many posts.

### 3. **Many-to-Many Relation** (`@ManyToMany`)

A `Many-to-Many` relationship exists when many entities are related to many others. For example, a `Student` can enroll in many `Courses`, and a `Course` can have many `Students`.

**Example**:
Let’s create `Student` and `Course` entities that have a many-to-many relationship.

```typescript
import {
  Entity,
  PrimaryGeneratedColumn,
  Column,
  ManyToMany,
  JoinTable,
} from "typeorm";

@Entity()
export class Student {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @ManyToMany(() => Course, (course) => course.students)
  @JoinTable()
  courses: Course[];
}

@Entity()
export class Course {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  title: string;

  @ManyToMany(() => Student, (student) => student.courses)
  students: Student[];
}
```

- `@ManyToMany` is defined on both entities, as each can have many of the other.
- `@JoinTable` is required on one side to specify this entity will own the relation. It defines the join table in the database that holds the relationships between `Student` and `Course`.

---

### How to Use Relations in NestJS

To use these relations in a NestJS application, ensure that:

1. You define entities as shown above.
2. In the services, you use `TypeORM` repository methods like `findOne()`, `find()`, and `save()` to manage relations.
3. You use the `relations` option in queries if you want to load related entities, like so:

```typescript
async findOneWithRelations(id: number): Promise<User> {
  return this.userRepository.findOne({
    where: { id },
    relations: ['profile', 'posts'],
  });
}
```

This query would return the user with their related profile and posts.

---

Each relation type allows for modeling different kinds of relationships in your database, giving flexibility to define how entities interact in your application. Depending on your use case, you can define the appropriate relationships and access them effectively using NestJS services.
