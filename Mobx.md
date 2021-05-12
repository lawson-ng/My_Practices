Mobx


# Mobx 🌱

![90ca180799ef4ffda8e9910fc267fe42](https://user-images.githubusercontent.com/72881474/117913761-249a3f00-b30c-11eb-8b5e-90dc4c94d26d.png)

## What is MobX ? 🤔
- Battle-tested library.
- Makes state management simple and scalable.

![ea6e2675d3e64534a8c354569f36fe55](https://user-images.githubusercontent.com/72881474/117913796-3aa7ff80-b30c-11eb-96ec-827cc3a7b27d.png)


## Who uses MobX ?
Refer: https://github.com/mobxjs/mobx/discussions/681

<img width="707" alt="87c359a0f69749df847726f4310e03fa" src="https://user-images.githubusercontent.com/72881474/117913855-5ca18200-b30c-11eb-8f52-5dfa00b6e1bd.png">

<img width="602" alt="74edaaeb21194f1787a21295ed867b3e" src="https://user-images.githubusercontent.com/72881474/117913943-86f33f80-b30c-11eb-963c-4537578c8a9f.png">
<img width="692" alt="0eb9a0402f16437ebe98805feb639ad4" src="https://user-images.githubusercontent.com/72881474/117914000-a38f7780-b30c-11eb-8003-21a05c0d9641.png">

<img width="487" alt="03fd39ca78584db0b56315d48109dad3" src="https://user-images.githubusercontent.com/72881474/117913962-94102e80-b30c-11eb-99e2-f4fe4c6686a8.png">


## Basic concepts

![f4f45039fa0440d197f53c46e87bf141](https://user-images.githubusercontent.com/72881474/117914085-c588fa00-b30c-11eb-8cb8-dc0f616a33ae.png)

1. First of all, there is the application state. Graphs of objects, arrays, primitives, references that forms the model of your application. These values are the “data cells” of your application.
2. Secondly there are derivations. Basically, any value that can be computed automatically from the state of your application. These derivations, or computed values, can range from simple values, like the number of unfinished todos, to complex stuff like a visual HTML representation of your todos. In spreadsheet terms: these are the formulas and charts of your application.
3. Reactions are very similar to derivations. The main difference is these functions don't produce a value. Instead, they run automatically to perform some task. Usually this is I/O related. They make sure that the DOM is updated or that network requests are made automatically at the right time.
4. Finally there are actions. Actions are all the things that alter the state. MobX will make sure that all changes to the application state caused by your actions are automatically processed by all derivations and reactions. Synchronously and glitch-free.

## Conclusion
#### `observable` decorator or observable(object or array) functions to make objects trackable for MobX
#### `computed`  decorator can be used to create functions that can automatically derive value from the state and cache them.
####  `autorun` to automatically run functions that depend on some observable state. This is useful for logging, making network requests, etc.
#### `observer` wrapper from the `mobx-react-lite` package to make your React components truly reactive. They will update automatically and efficiently. Even when used in large complex applications with large amounts of data.
````javascript
class ObservableTodoStore {
  todos = [];
  pendingRequests = 0;

  constructor() {
    makeObservable(this, {
      todos: observable,
      pendingRequests: observable,
      completedTodosCount: computed,
      report: computed,
      addTodo: action,
    });
    autorun(() => console.log(this.report));
  }

  get completedTodosCount() {
    return this.todos.filter(
      todo => todo.completed === true
    ).length;
  }

  get report() {
    if (this.todos.length === 0)
      return "<none>";
    const nextTodo = this.todos.find(todo => todo.completed === false);
    return `Next todo: "${nextTodo ? nextTodo.task : "<none>"}". ` +
      `Progress: ${this.completedTodosCount}/${this.todos.length}`;
  }

  addTodo(task) {
    this.todos.push({
      task: task,
      completed: false,
      assignee: null
    });
  }
}
````

````javascript
const peopleStore = observable([
  { name: "Michel" },
  { name: "Me" }
]);
observableTodoStore.todos[0].assignee = peopleStore[0];
observableTodoStore.todos[1].assignee = peopleStore[1];
peopleStore[0].name = "Michel Weststrate";
````
