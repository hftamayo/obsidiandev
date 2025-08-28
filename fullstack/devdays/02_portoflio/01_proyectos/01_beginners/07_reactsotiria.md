
# Project Status & Migration Roadmap

## Version 0.0.1-main, ver commit: https://github.com/hftamayo/reactsotiria/pull/8


## Current State Analysis

### ✅ Working Components
- **Authentication System**: Redux auth slice with login/logout functionality
- **Routing**: React Router setup with entity-based routes (`/entities/roles`, `/entities/tasks`)
- **Redux Store**: Basic structure with slices for auth, roles, and tasks
- **API Client**: Axios-based client with error handling
- **Metadata System**: Entity-specific metadata configuration (roleMetadata, taskMetadata)

### ❌ Critical Issues
- **DataTable Component**: Multiple "Unknown entity type" errors in DataTableHeader/DataTableRow
- **Type System**: Inconsistent AsyncThunk typing between components
- **Data Fetching**: Race conditions, infinite re-renders, and "Too Many Requests" errors
- **State Management**: Redux thunks not properly integrated with UI components
- **Error Handling**: Generic error states without proper user feedback

### 🔧 Technical Debt
- **Custom CRUD Implementation**: Fragile, project-specific, hard to maintain
- **Type Safety**: Multiple type assertion workarounds (`as any`)
- **Performance**: No virtualization, inefficient re-renders
- **Testing**: No test coverage for critical components

## Recommended Migration Path

### Phase 1: Stabilize Current System (Week 1-2)
1. **Fix Critical Bugs**
   - Resolve "Unknown entity type" errors in DataTable components
   - Fix AsyncThunk type inconsistencies
   - Implement proper error boundaries

2. **Clean Up Types**
   - Remove `AppAsyncThunkAction` wrapper
   - Standardize AsyncThunk usage across all slices
   - Fix entity type mapping in `mapToStandardRecord`

3. **Data Fetching Stability**
   - Implement proper loading states
   - Add request deduplication
   - Fix race conditions in useEffect hooks

### Phase 2: Modernize Data Layer (Week 3-4)
1. **Migrate to React Query**
   - Replace Redux thunks with React Query hooks
   - Implement proper caching and background updates
   - Add optimistic updates for better UX

2. **API Response Standardization**
   - Create consistent API envelope types
   - Implement proper error handling with user-friendly messages
   - Add request/response logging for debugging

### Phase 3: Implement TanStack Table (Week 5-6)
1. **Install Dependencies**
   ```bash
   npm install @tanstack/react-table @tanstack/react-query
   ```

2. **Create Table Wrapper**
   - Build generic table component using TanStack Table
   - Create metadata-to-columns mapper
   - Implement server-side pagination

3. **Migrate Existing Tables**
   - Convert role and task tables to new system
   - Maintain existing metadata structure
   - Add sorting, filtering, and pagination

### Phase 4: Extract Reusable Library (Week 7-8)
1. **Package Structure**
   ```
   @company/crud-table/
   ├── src/
   │   ├── components/
   │   │   ├── DataTable.tsx
   │   │   ├── DataTableHeader.tsx
   │   │   └── DataTableRow.tsx
   │   ├── hooks/
   │   │   ├── useDataTable.ts
   │   │   └── useServerPagination.ts
   │   ├── types/
   │   │   ├── metadata.types.ts
   │   │   └── table.types.ts
   │   └── utils/
   │       ├── columnMapper.ts
   │       └── dataAdapter.ts
   ```

2. **Publish Internal Package**
   - Extract table components and utilities
   - Create comprehensive documentation
   - Set up CI/CD for package updates

## Immediate Action Items

### This Week
- [ ] Fix "Unknown entity type" errors in DataTableHeader.tsx
- [ ] Resolve AsyncThunk type inconsistencies
- [ ] Implement proper loading states

### Next Week
- [ ] Set up React Query
- [ ] Create TanStack Table wrapper
- [ ] Begin migration of existing tables

## Success Metrics
- **Stability**: Zero runtime errors in table components
- **Performance**: <100ms table render time for 1000+ rows
- **Reusability**: Table component works across different projects
- **Developer Experience**: <5 minutes to add new entity table

## Risk Mitigation
- **Backward Compatibility**: Maintain existing metadata structure during migration
- **Incremental Migration**: Migrate one table at a time to minimize risk
- **Rollback Plan**: Keep old implementation until new system is fully tested
- **Team Training**: Document new patterns and provide examples

This migration will transform a fragile, project-specific CRUD system into a robust, reusable table library that can serve multiple projects and teams.