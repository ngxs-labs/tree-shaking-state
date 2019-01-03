## Current behavior

```ts
// todo.state.ts

@State<string[]>({
  name: 'todo',
  defaults: []
})
export class TodoState {
 ...
}

@State<TodoStateModel>({
  name: 'todos',
  defaults: {
    todo: [],
    pizza: { model: undefined }
  },
  children: [TodoState]
})
export class TodosState {
  ...
}


// app.module.ts

@NgModule({
  imports: [ NgxsModule.forRoot([TodosState, TodoState]) ]
})
export class AppModule {}
```

## Expected behavior

```ts
// todo.state.ts

@State<string[]>({
  name: 'todo',
  defaults: []
})
export class TodoState {
 ...
}

@State<TodoStateModel>({
  name: 'todos',
  defaults: {
    todo: [],
    pizza: { model: undefined }
  },
  children: [TodoState], // default -> providedIn: 'ngxsRoot' for TodoState
  providedIn: 'ngxsRoot'
})
export class TodosState {
  ...
}


// app.module.ts

@NgModule({
  imports: [ NgxsModule.forRoot() ]
})
export class AppModule {}
```

## What is the motivation / use case for changing the behavior?

![image](https://user-images.githubusercontent.com/12021443/47605274-1b2a7f00-da0d-11e8-9e04-231c5840ac4b.png)

```ts
/**
 * Options that can be provided to the store.
 */
export interface StoreOptions<T> {
  /**
   * Name of the state. Required.
   */
  name: string;

  /**
   * Default values for the state. If not provided, uses empty object.
   */
  defaults?: T;

  /**
   * Sub states for the given state.
   */
  children?: any[];

  /**
   * Define states in your root module
   * */
  providedIn?: Type<any> | 'ngxsRoot' | null;
}
```
