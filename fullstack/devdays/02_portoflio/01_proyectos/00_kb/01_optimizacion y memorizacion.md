
To address the performance issue, we can take several steps to identify and optimize potential bottlenecks in your React application. Here are some suggestions:

### Step 1: Analyze Performance with React DevTools

Use the React DevTools Profiler to analyze the performance of your application. This tool can help you identify which components are taking the most time to render and why.

### Step 2: Optimize Component Rendering

Ensure that your components are only re-rendering when necessary. You can use `React.memo` to memoize functional components and `useMemo` or `useCallback` hooks to memoize values and functions.

### Step 3: Lazy Load Components

Consider lazy loading components that are not immediately needed. This can help reduce the initial load time of your application.

### Step 4: Optimize State Management

Ensure that your state management is efficient and that you are not causing unnecessary re-renders.

### Step 5: Optimize CSS and Styles

Ensure that your CSS and styles are optimized and not causing performance issues.

### Step 6: Optimize Network Requests

Ensure that your network requests are optimized and not causing delays in rendering.

### Example Refactor

Here is an example refactor of your `MainLayout` component to include some of these optimizations:

**MainLayout.tsx**
```tsx
import React, { useState, useEffect, useMemo, useCallback } from 'react';
import { useAppSelector } from '@/shared/services/redux/hooks/useAppSelector';
import { CustomOutlet } from '@/shared/services/routing/CustomOutlet';
import { DashBoardHeader } from '@/features/dashboard/components/header/DashBoardHeader';
import { DashBoardToggleMenuBar } from '@/features/dashboard/components/menu/DashBoardToggleMenuBar';
import { DashBoardFooter } from '@/features/dashboard/components/footer/DashBoardFooter';
import { menuOptions } from '@/features/dashboard/components/menu/config/menuOptions';
import { APP_NAME } from '@/shared/utils/envvars';
import { MainLayoutStyles } from '@/shared/utils/twind/styles';

export const MainLayout: React.FC = React.memo(() => {
  const theme = useAppSelector((state: any) => state.theme.theme);
  const isAuthenticated = true;
  const userRole = 'admin';
  const { adminMenuOptions, supervisorMenuOptions, userMenuOptions } = menuOptions();

  const [sidebarToggle, setSidebarToggle] = useState(true);

  useEffect(() => {
    document.documentElement.classList.toggle('dark', theme === 'dark');
  }, [theme]);

  const selectMenuOptions = useMemo(() => {
    if (userRole === 'admin') {
      return adminMenuOptions;
    } else if (userRole === 'supervisor') {
      return supervisorMenuOptions;
    } else {
      return userMenuOptions;
    }
  }, [userRole, adminMenuOptions, supervisorMenuOptions, userMenuOptions]);

  const handleSidebarToggle = useCallback(() => {
    setSidebarToggle((prev) => !prev);
  }, []);

  return (
    <div className={MainLayoutStyles.layoutContainer}>
      <div className={MainLayoutStyles.layoutHeader}>
        {isAuthenticated ? (
          <DashBoardHeader
            setSidebarToggle={handleSidebarToggle}
            appName={APP_NAME}
          />
        ) : null}
      </div>
      <div className="flex flex-grow flex-1">
        <DashBoardToggleMenuBar
          options={selectMenuOptions}
          isCollapsed={sidebarToggle}
        />
        <div
          className={`${MainLayoutStyles.layoutContent(sidebarToggle)} flex-1`}
        >
          <div className="flex-1">
            <CustomOutlet />
          </div>
        </div>
      </div>
      <div className={MainLayoutStyles.layoutFooter}>
        {isAuthenticated ? <DashBoardFooter /> : null}
      </div>
    </div>
  );
});
```

### Explanation

- **Memoization**: Use `React.memo` to memoize the `MainLayout` component, preventing unnecessary re-renders. Use `useMemo` to memoize the `selectMenuOptions` value and `useCallback` to memoize the `handleSidebarToggle` function.
- **Lazy Loading**: Consider lazy loading components that are not immediately needed using `React.lazy` and `Suspense`.

### Summary

By analyzing the performance with React DevTools and applying optimizations such as memoization, lazy loading, and efficient state management, you can improve the performance of your React application. If you have any more questions or need further assistance, feel free to ask!