
```
- pnpm run test: ejecuta toda la suite de testing
- pnpm run test: coverage -> code coverage
```

Your test script is:

nx run-many -t vitest:test --all --passWithNoTests

That means Nx is orchestrating the workspace tests across all projects, using each project’s Vitest target/config. It is usually the more reliable script for “run the whole monorepo test suite consistently.”

Your coverage script is:

vitest run --coverage

That runs Vitest directly, outside Nx. It is the right entry point for collecting coverage, but it is also more sensitive to whatever Vitest defaults to in that direct mode, so it can behave differently from the Nx test target.

So the practical answer is:

- For running the repo tests consistently: the Nx test script is usually more reliable.
- For generating coverage: the coverage script is the one that matters, but it is not automatically more reliable.
- If they behave differently, that usually means config/environment drift between Nx-invoked Vitest and direct Vitest.

In your case, the document-related error strongly suggests that the coverage path is using a different test environment than the Nx test path.



### problema del test coverage

 FAIL  apps/main-app/src/components/company/hooks/useDeleteSelectedCompany.spec.ts > useDeleteSelectedCompany > reports errors via handl
eFrontendError                                                                                                                          
ReferenceError: document is not defined                                                                                                 
 ❯ render node_modules/.pnpm/@testing-library+react@16.3.0_@testing-library+dom@10.4.0_@types+react-dom@19.2.3_@types+reac_gyndm7swnpmbe
rufkiuzfyam4y/node_modules/@testing-library/react/dist/pure.js:257:5                                                                    
 ❯ Proxy.renderHook node_modules/.pnpm/@testing-library+react@16.3.0_@testing-library+dom@10.4.0_@types+react-dom@19.2.3_@types+reac_gyn
dm7swnpmberufkiuzfyam4y/node_modules/@testing-library/react/dist/pure.js:340:7                                                          
 ❯ apps/main-app/src/components/company/hooks/useDeleteSelectedCompany.spec.ts:142:5                                                    
    140|     );                                                                                                                         
    141|                                                                                                                                
    142|     renderHook(() => useDeleteSelectedCompany());                                                                              
       |     ^                                                                                                                          
    143|                                                                                                                                
    144|     await waitFor(() => {     
    
    
 That specific error means the test is running without a browser-like environment. So the underlying issue is not coverage itself, but that the coverage run is using a different test environment or config than your normal test run. In other words, coverage is exposing an environment mismatch, not creating a new kind of problem.

If you want, I can next tell you the most likely config difference to check first, without running anything.   