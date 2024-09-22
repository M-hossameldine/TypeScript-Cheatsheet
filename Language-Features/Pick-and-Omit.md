# Pick and Omit

Pick and Omit are utility types available in TypeScript since version 2.1 and 3.5 respectively.

#### Pick Example

Constructs a type made up of as subset of the properties of another type:

```
interface Person {
  firstName: string,
  lastName: string,
  age: number,
  email: string,
  phone: string,
}

type ContactDetails = Pick<Person, "email" | "phone>;

const a : ContactDetails = { email: "hossam@mail.com", phone: "01244588489"};
```

#### Omit Example

Works similarly but you declare the properties you don't want:

```
interface ExamEntry {
  name: string,
  candidateId: string,
  answers: boolean[],
  foundationalPaper: boolean,
  completionMins: number,
}

type MarkingDetails = Omit<ExamEntry, 'name'>;

const entry : MarkingDetails = {
  candidateId: "42",
  answers: [true, false, false, true, false, true, true],
  foundationPaper: false,
  completionMins: 35,
}
```

### Use Cases

---

Both Pick and Omit are useful when you want to create a new type based on an existing type but with a subset of the properties.

1. **Define the types exported from your API or library.**
   You can use Pick to define a type that only includes the properties you want to expose.

```
  type User = {
    id: number;
    firstName: string;
    lastName: string;
    hashedPassword: string;
    createdAt: string;
  }

  type Prospect = {
    id: number;
    firstName: string;
    lastName: string;
    referrer: string;
  }

  export type RenderedUser = Pick<User, 'firstName' | 'lastName' >;
  export type RenderedProspect = Omit<Prospect, 'id' >;
```

2. **Typing field transformations.**
   You can:
   - Use Omit to define a type that excludes the properties you want to transform.
   - Use Omit and override the properties you want to transform.
   - Use Pick to define a type that only includes the properties you want to transform.

```
  type RenderedUser2 = Omit<User, 'createdAt' | 'hashedPassword' > & {
    createdAt: Date;
  }

  // * Transform User createdAt field to Date
  declare function hydrateUsr(r: User): RenderedUser2;

  // * Omit createdAt and updatedAt fields from the passed object of type T
  declare function removeDate<
    T extends { createdAt: Date, updatedAt: Date }
  >(obj: T): Omit<T, 'createdAt' | 'updatedAt'>;
```

3. **Flexible function signatures (with clear primary intensions)**
   You can use it as a documentation and future reference. You can use Pick to define a type that only includes the properties you want to pass to a function.

```
  declare const u: User;
  declare const p: Prospect;

  // the sendMarketingEmails function doesn't need to receive the whole User object
  function sendMarketingEmails(recipient: User) {
    // ... some logic where you only need user's firstName and lastName
  }

  // this could be a sort of future documentation to reference for the User type for any future changes
  function sendMarketingEmailsRefactored(recipient: Pick<User, 'firstName' | 'lastName'>) {
    // ... you only received the fields that you need
  }
```

### References

---

- [Typescript Tips 1: Pick & Omit - By Joe Rackham](https://joerackham.medium.com/typescript-tips-1-pick-omit-f18c7977fc4a)
