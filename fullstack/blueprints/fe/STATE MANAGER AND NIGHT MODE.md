
For a theme/night-mode feature, Redux Toolkit is usually too much unless your app already uses Redux heavily or the theme state is deeply tied to other global application behavior.
Better default option
Use a small, modular theme feature with:
a ThemeProvider
a useTheme() hook
persistence in localStorage
applying a class/attribute to the root element, e.g. document.documentElement.classList.toggle("dark")
optionally reading system preference with prefers-color-scheme
That is enough for most React apps.


For microfrontends, a better modular approach is to define a small theme contract:

type ThemeMode = "light" | "dark" | "system";

interface ThemeController {
  mode: ThemeMode;
  resolvedTheme: "light" | "dark";
  setMode: (mode: ThemeMode) => void;
  toggleTheme: () => void;
}

Then the current monorepo implementation can use React Context, Zustand, or even plain browser APIs. Later, microfrontends can communicate through a shared shell, custom events, or shared storage without being forced into Redux.

features/theme/
  domain/
    theme.contracts.ts
  application/
    theme.store.ts
    theme.effects.ts
  ui/
    useTheme.ts
    ThemeProvider.tsx
    ThemeToggle.tsx
    
const { mode, resolvedTheme, toggleTheme } = useTheme();

Microfrontend-friendly recommendation
Expose theme changes through simple web/platform contracts, not Redux-specific contracts:
CSS variable/theme class on document.documentElement
localStorage key such as app.theme
custom browser event such as app:theme-change
optional shell-level theme provider
Example contract:

window.dispatchEvent(
  new CustomEvent("app:theme-change", {
    detail: {
      mode: "dark",
      resolvedTheme: "dark",
    },
  }),
);

Keep the public API small and implementation-agnostic so it can later be moved into a shared shell or reused by microfrontends without coupling everything to Redux.

The point of the contract is to avoid making the rest of your app, and future microfrontends, depend directly on the internal state manager implementation.
So:
Use Zustand internally, expose a theme contract externally.


first version of the contract:
export type ThemeMode = "light" | "dark" | "system";

export type ResolvedTheme = "light" | "dark";

export interface ThemeState {
  mode: ThemeMode;
  resolvedTheme: ResolvedTheme;
}

export interface ThemeActions {
  setMode: (mode: ThemeMode) => void;
  toggleTheme: () => void;
}

export type ThemeStore = ThemeState & ThemeActions;

Zustand sotre:
const useThemeStore = create<ThemeStore>()(...)

Custom hook:
export function useTheme() {
  return useThemeStore((state) => ({
    mode: state.mode,
    resolvedTheme: state.resolvedTheme,
    setMode: state.setMode,
    toggleTheme: state.toggleTheme,
  }));
}

For future microfrontends
For microfrontends, the “contract” should be even more platform-friendly.
Zustand is great inside one React app/module, but separate microfrontends may not share:
the same React tree
the same Zustand instance
the same bundle/runtime
the same dependency versions

So for cross-microfrontend communication, expose theme through browser-level contracts such as:
DOM class or attribute
<html data-theme="dark">
or

<html class="dark">

localStorage:
localStorage.setItem("app.theme.mode", "dark");

Expose Zustand store like this:
import { useTheme } from "@/shared/theme";

Tree execution:
Theme feature public API
        |
        v
useTheme() hook
        |
        v
Zustand store internally
        |
        v
Side effects:
- update <html data-theme="...">
- persist to localStorage
- emit "app:theme-change"

So when the user toggles theme:
Zustand updates internal app state.
The app updates the root HTML theme.
The selected theme is saved to localStorage.
A browser event is emitted for future microfrontends.
    