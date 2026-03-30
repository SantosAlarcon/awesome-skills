# React Component Patterns

## Description
Common patterns for building reusable, maintainable React components with proper prop typing and composition.

## When to Use
- Creating new UI components
- Refactoring class components to functional
- Building component libraries
- Handling component composition and reusability

## How to Use

1. **Start with functional components**: Use hooks for state and side effects
2. **Define prop types explicitly**: Use TypeScript interfaces for props
3. **Prefer composition over prop drilling**: Use children or render props
4. **Extract custom hooks**: Share logic between components
5. **Memoize appropriately**: Use `useMemo` and `useCallback` when needed

## Examples

### Typed Props with Interface
```tsx
interface ButtonProps {
  variant: 'primary' | 'secondary';
  size?: 'sm' | 'md' | 'lg';
  children: React.ReactNode;
  onClick?: () => void;
  disabled?: boolean;
}

export function Button({ variant, size = 'md', children, onClick, disabled }: ButtonProps) {
  return (
    <button className={`btn btn-${variant} btn-${size}`} onClick={onClick} disabled={disabled}>
      {children}
    </button>
  );
}
```

### Compound Components Pattern
```tsx
interface TabsContextValue {
  activeTab: string;
  setActiveTab: (id: string) => void;
}

const TabsContext = React.createContext<TabsContextValue | null>(null);

function Tabs({ children, defaultTab }: { children: React.ReactNode; defaultTab: string }) {
  const [activeTab, setActiveTab] = useState(defaultTab);
  return (
    <TabsContext.Provider value={{ activeTab, setActiveTab }}>
      <div className="tabs">{children}</div>
    </TabsContext.Provider>
  );
}

function TabList({ children }: { children: React.ReactNode }) {
  return <div role="tablist">{children}</div>;
}

function Tab({ id, children }: { id: string; children: React.ReactNode }) {
  const { activeTab, setActiveTab } = useContext(TabsContext)!;
  return (
    <button role="tab" aria-selected={activeTab === id} onClick={() => setActiveTab(id)}>
      {children}
    </button>
  );
}

Tabs.List = TabList;
Tabs.Tab = Tab;
```

### Custom Hook for Data Fetching
```tsx
function useUser(userId: string) {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    setLoading(true);
    fetchUser(userId)
      .then(setUser)
      .catch(setError)
      .finally(() => setLoading(false));
  }, [userId]);

  return { user, loading, error };
}
```

## Best Practices

- Keep components focused on a single responsibility
- Extract complex logic into custom hooks
- Use `React.memo()` only when profiling shows re-renders are costly
- Co-locate styles, tests, and types with components when possible
- Export types separately from components for better reusability

## Related Skills
- [Next.js Patterns](../nextjs/) — Server components and routing patterns
