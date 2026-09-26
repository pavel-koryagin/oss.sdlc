# Code Standards

Examples are in TypeScript, but the principles we state here are for every language we use.

## Comments as an Algorithm Outline

Add a short comment to every logical block. Reading only the comments should reveal the algorithm.

```ts
// Find incomplete todos
const incompleteTodos = todos.filter(todo => !todo.completed);

// Complete each todo
for (const todo of incompleteTodos) {
  todo.completed = true;
}

// Return the completed todos
return incompleteTodos;
```

Comment functions the same way. JSDoc-like blocks are allowed but not required. Maintain them when they exist or are requested. Otherwise, describe the function's specific job in one sentence.

```ts
// Add a todo unless one with the same title already exists.
function addUniqueTodo(todos: Todo[], title: string): void {
  // Check whether the todo exists
  const exists = todos.some(todo => todo.title === title);

  // Add the todo when it is new
  if (!exists) {
    todos.push({ title, completed: false });
  }
}
```

## Naming

- Function names start with a verb
  - Exception: when functions work as DSL operators to define a structure, they are nouns

## Decomposition

Apply wisely, do not miss when a rule seems relevant but semantically is not.

- Extract complex deterministic logic into pure functions in their own files
- Gravitate to implementing functions as pure
- Gravitate to naming a file matching the name of its main function or type

## Tests

Exception: do not apply these rules to an E2E Playwright test – their style is completely independent of this. If you work on a Playwright test, you must be provided with a separate playbook for that.

### Follow AAA

Format tests as Arrange, Act, Assert.

```ts
// Arrange
const todos = new TodoList(['Buy milk', 'Wash car']);
todos.complete('Wash car');

// Act
const result = todos.getIncomplete();

// Assert
expect(result).toEqual(['Buy milk']);
```

```ts
// Arrange
const todos = new TodoList();

// Act
todos.add('Buy milk');

// Assert
expect(todos.items).toEqual(['Buy milk']);
```

For expected errors, define the operation as a function in Act and capture its error for granular assertions. The function wrapping and the act code should be on different lines.

Implement `expectToThrow(ExpectedType, act)` as a test helper that returns the thrown error typed as `ExpectedType` and rethrows errors of any other type.

```ts
// Arrange
const todos = new TodoList();

// Act
const act = () => {
  todos.add('');
};

// Assert
const error = await expectToThrow(InvalidTodoError, act);
expect(error.message).toBe('Todo title is required');
expect(error.code).toBe('required_field');
```

Arrange may be omitted when it is unnecessary.

#### Good and Bad

Bad. Overkill.

```ts
// Arrange
const a = 1;
const b = 1;

// Act
const result = compare(a, b);

// Assert
expect(result).toBe(true);
```

Good. Omit the Arrange block when you have nothing to say. Include the constant literals into the signature of a call we illustrate with this test.

```ts
// Act
const result = compare(1, 1);

// Assert
expect(result).toBe(true);
```

### AAA-style Scenarios

Scenario tests allow illustrate the real-life scenarios with logical connection between their steps.

Stick to the AAA style. Mark every phase with comments, for example: `Arrange, Act, Assert, Act, Assert, Arrange, Act, Assert`. In multi-act tests, label every Act with its step name, such as `Act: add a todo`.

```ts
// Arrange
const todos = new TodoList();

// Act: Add a todo
const addedTodo = todos.add('Buy milk');

// Assert
expect(todos.get(addedTodo.id)).toMatchObject({ title: 'Buy milk', completed: false });

// Act: Complete the todo
todos.complete(addedTodo.id);
const todoAfterCompletion = todos.get(addedTodo.id);

// Assert
expect(todoAfterCompletion).toEqual({ ...addedTodo, completed: true });
```

### AAA Exceptions

Multiple single-line assertions are allowed for pure formatting functions when they make cases easy to compare. No block comments are required in such test.

```ts
expect(formatTodoCount(0)).toBe('No todos');
expect(formatTodoCount(1)).toBe('1 todo');
expect(formatTodoCount(2)).toBe('2 todos');
```

