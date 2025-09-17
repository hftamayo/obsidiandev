
## Dir structure:

src/domains/todo/
├── TodoModule.tsx          # Main module entry
├── hooks/
│   ├── useTodoOperations.ts
│   ├── useTodoQueries.ts
│   └── useTodoMutations.ts
├── components/
│   ├── TodoList.tsx
│   ├── TodoForm.tsx
│   └── TodoItem.tsx
├── services/
│   └── TodoService.ts
└── types/
    └── Todo.types.ts


## Example of Domain model:

```
// src/domains/todo/index.ts
export { TodoModule } from './TodoModule';
export { useTodoOperations } from './hooks/useTodoOperations';
export { TodoList } from './components/TodoList';
export { TodoForm } from './components/TodoForm';

// src/domains/todo/TodoModule.tsx
export const TodoModule: React.FC = () => {
  return (
    <TodoProvider>
      <TodoList />
      <TodoForm />
    </TodoProvider>
  );
};

```

## Example of module federation setup:

```

// webpack.config.js
const ModuleFederationPlugin = require('@module-federation/webpack');

module.exports = {
  plugins: [
    new ModuleFederationPlugin({
      name: 'todoModule',
      filename: 'remoteEntry.js',
      exposes: {
        './TodoModule': './src/domains/todo/TodoModule',
        './useTodoOperations': './src/domains/todo/hooks/useTodoOperations',
      },
      shared: {
        react: { singleton: true },
        'react-dom': { singleton: true },
      },
    }),
  ],
};

```

## Example of microfrontend integration:

```

// Main app consuming the microfrontend
const TodoModule = React.lazy(() => import('todoModule/TodoModule'));

export const App = () => {
  return (
    <Suspense fallback={<Loading />}>
      <TodoModule />
    </Suspense>
  );
};

```

## Custom hooks para el business logic:

```
// src/domains/todo/hooks/useTodoOperations.ts
export const useTodoOperations = () => {
  // All todo business logic here
  // No UI concerns
  // Easy to test and reuse
};

```


## Tools:

1. build tool: Vite -> WebPack5
2. package manager: yarn -> pnpm
3. Testing: Jest -> Jest + Module Federation support
4. project structure: monorepo setup
5. additional tools: Prettier, ESLint, Storybook (component development), Lerna or Nx (monorepo orchestation)