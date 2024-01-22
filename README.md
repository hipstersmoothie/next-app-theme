# next-app-theme

Simple theming for Next.js apps using the App Directory.

- No flashing on page load
- Works with tailwind

> NOTE: You can actually use this package outside of Next.js. In essence it'js just a script that executes in the head of your page. So you can just use `ThemeScript` and the global `__setPreferredTheme`.

## Installation

```bash
npm install next-app-theme
```

## Usage

First add the script to your root layout.
Make sure to add it to the `<head>` of your page so that it runs before your app.

`app/layout.js`:

```tsx
import { ThemeScript } from "next-app-theme/theme-script";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" suppressHydrationWarning>
      <head>
        <ThemeScript />
      </head>
      <body>{children}</body>
    </html>
  );
}
```

Then use the theme in your `app/global.css`:

```css
.light {
  color-scheme: light;
}

.dark {
  color-scheme: dark;
}
```

Then create a component to toggle the theme:

```tsx
"use client";

import { useTheme } from "next-app-theme/use-theme";
import { Sun, Moon } from "lucide-react";

function ThemeToggle() {
  const { theme, toggleThem } = useTheme();
  const icon = theme === "dark" ? <Sun /> : <Moon />;

  return <button onClick={toggleThem}>{icon}</button>;
}
```

Then use the toggle somewhere in your app:

```tsx
// Don't SSR the toggle since the value on the server will be different than the client
const SetThemeButton = dynamic(() => import("./ui/SetThemeButton"), {
  ssr: false,
  // Make sure to code a placeholder so the UI doesn't jump when the component loads
  loading: () => <div className="w-9 h-9" />,
});

export function Header() {
  return (
    <header className="flex justify-between items-center">
      <h1 className="text-2xl font-bold">My App</h1>
      <SetThemeButton />
    </header>
  );
}
```

## Inspiration

- [This discussion reply](https://github.com/vercel/next.js/discussions/53063#discussioncomment-6996549)
- https://hangindev.com/blog/avoid-flash-of-default-theme-an-implementation-of-dark-mode-in-react-app
