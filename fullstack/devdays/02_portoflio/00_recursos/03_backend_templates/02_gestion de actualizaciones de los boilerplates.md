
No, changes made to a boilerplate repository will not automatically propagate to all repositories that were created from that template. Each repository created from the template is a separate, independent repository. 

### Options to Manage Updates:

1. **Manual Updates**:
   - You can manually apply updates to each repository created from the template. This can be time-consuming and error-prone, especially if you have many repositories.

2. **Using Git Submodules**:
   - You can use Git submodules to include the boilerplate as a submodule in your projects. This way, you can update the submodule in each project to pull in changes from the boilerplate repository.

3. **Using a Monorepo**:
   - You can manage multiple projects within a single monorepo using tools like Lerna or Nx. This allows you to share code and configurations across projects more easily.

4. **Custom Scripts**:
   - You can write custom scripts to automate the process of pulling updates from the boilerplate repository and applying them to your projects.

### Example Using Git Submodules:

1. **Add Boilerplate as a Submodule**:
   - In your project repository, add the boilerplate repository as a submodule.

   ```sh
   git submodule add https://github.com/yourusername/boilerplate-repo.git boilerplate
   ```

2. **Update Submodule**:
   - When you make changes to the boilerplate repository, you can update the submodule in your project repository to pull in the latest changes.

   ```sh
   cd boilerplate
   git pull origin main
   cd ..
   git add boilerplate
   git commit -m "Update boilerplate submodule"
   git push origin main
   ```

### Summary:

- **Manual Updates**: Apply updates manually to each repository.
- **Git Submodules**: Use submodules to include the boilerplate in your projects and update it as needed.
- **Monorepo**: Manage multiple projects within a single repository.
- **Custom Scripts**: Automate the update process with custom scripts.

Choose the approach that best fits your workflow and project requirements.


## Poniendo en practica estos pasos

### Previa

1. desde github hice un repo basado en el template tb_fe_react_boilerplate
2. clone el repo a mi equipo
3. hice un npm install

### Agregando el template como submodulo

![[Pasted image 20241010090709.png]]

![[Pasted image 20241010090817.png]]

![[Pasted image 20241010091210.png]]

==vergon, aca puedo resumir el proceso asi: tengo 2 veces el repo, una en ROOTDIR y la otra en Template

___
You have understood the process correctly. Here are the answers to your concerns:

1. **Checking for updates**: Yes, you will need to navigate to the [`template`](command:_github.copilot.openRelativePath?%5B%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Freactjs%2Fmyprojects%2Fbuddyman%2Fbuddyman_react_microshared%2Ftemplate%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%2278359f25-273e-4aef-adad-a9ae1d166add%22%5D "/home/hftamayo/sourcecode/reactjs/myprojects/buddyman/buddyman_react_microshared/template") directory and execute a `git pull` to fetch updates from the `tb_fe_react_boilerplate` repository.

2. **Manual updates**: Correct, you will need to manually identify and apply the changes from the [`template`](command:_github.copilot.openRelativePath?%5B%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Freactjs%2Fmyprojects%2Fbuddyman%2Fbuddyman_react_microshared%2Ftemplate%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%2278359f25-273e-4aef-adad-a9ae1d166add%22%5D "/home/hftamayo/sourcecode/reactjs/myprojects/buddyman/buddyman_react_microshared/template") directory to your root directory (`ROOTDIR`). This can be error-prone and may require careful merging to avoid inconsistencies.

3. **Impact on performance and code duplication**:
   - **Performance**: Having duplicate files and directories in your repository can increase the size of your repository, which might slightly impact performance, especially during cloning or pulling changes. However, it should not significantly impact the runtime performance of your application.
   - **Code duplication**: Yes, you will have two versions of your configuration files and directory structures. This can lead to confusion and potential errors if the files are not kept in sync.
   - **Deployment size**: The size of your deployed application might increase if you include the [`template`](command:_github.copilot.openRelativePath?%5B%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Freactjs%2Fmyprojects%2Fbuddyman%2Fbuddyman_react_microshared%2Ftemplate%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%2278359f25-273e-4aef-adad-a9ae1d166add%22%5D "/home/hftamayo/sourcecode/reactjs/myprojects/buddyman/buddyman_react_microshared/template") directory in your deployment package. You can mitigate this by excluding the [`template`](command:_github.copilot.openRelativePath?%5B%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Freactjs%2Fmyprojects%2Fbuddyman%2Fbuddyman_react_microshared%2Ftemplate%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%2278359f25-273e-4aef-adad-a9ae1d166add%22%5D "/home/hftamayo/sourcecode/reactjs/myprojects/buddyman/buddyman_react_microshared/template") directory from your deployment process.
   - **CI pipelines**: Having duplicate files might trigger false positives in CI pipelines, especially if the pipelines are set up to check for consistency or run tests on all files. You can configure your CI pipelines to exclude the [`template`](command:_github.copilot.openRelativePath?%5B%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Freactjs%2Fmyprojects%2Fbuddyman%2Fbuddyman_react_microshared%2Ftemplate%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%2C%2278359f25-273e-4aef-adad-a9ae1d166add%22%5D "/home/hftamayo/sourcecode/reactjs/myprojects/buddyman/buddyman_react_microshared/template") directory to avoid this issue.

To summarize, while using a submodule can help you keep track of updates from the template repository, it does introduce some complexity and potential issues related to code duplication and manual merging. You will need to carefully manage these aspects to ensure a smooth development and deployment process.

___
### Evaluando la opcion de monorepo vs polirepo

==got it bro, so, with the intention to find a better solution I will try to summarize what I need to do:
1. to design a boilerplate for my production ready and microservices oriented projects
2. create my github repos based on this template
3. keep track of the changes from the template and apply them transparently without affecting my source code
4. avoid code duplication, this means: one single please -ROOTDIR- for my template related files and files exclusive of the current project

is that possible? (please do not include source code)



For production-ready applications and effective management of development teams, here are some key considerations and recommendations:

### Recommendations

1. **Monorepo with Proper Tooling**:
   - **Pros**: Centralized management, easier code sharing, consistent updates, and simplified dependency management.
   - **Cons**: Can become complex as the number of services grows.
   - **Tools**: Use tools like [Lerna](https://lerna.js.org/), [Nx](https://nx.dev/), or [Bazel](https://bazel.build/) to manage the complexity. These tools help with dependency management, build optimization, and project organization.

2. **Polyrepo with Shared Libraries**:
   - **Pros**: Clear separation of concerns, easier to scale teams, and independent versioning and deployment.
   - **Cons**: More effort to keep shared libraries in sync across repositories.
   - **Tools**: Use a package manager like [npm](https://www.npmjs.com/) or [Yarn](https://yarnpkg.com/) to manage shared libraries. Set up CI/CD pipelines to automate the process of updating and deploying shared libraries.

### Key Considerations

1. **Team Structure**:
   - **Monorepo**: Suitable if you have a smaller team or teams that need to collaborate closely. It simplifies dependency management and ensures consistency.
   - **Polyrepo**: Better for larger teams or teams that need to work independently. It allows for more flexibility and autonomy.

2. **Deployment Strategy**:
   - **Monorepo**: Use CI/CD pipelines to handle the deployment of individual microservices. Ensure that changes in one service do not affect others unless intended.
   - **Polyrepo**: Each repository can have its own CI/CD pipeline, allowing for independent deployment and scaling.

3. **Code Quality and Consistency**:
   - **Monorepo**: Easier to enforce coding standards and practices across all services. Use linters, code formatters, and automated tests to maintain quality.
   - **Polyrepo**: Requires more effort to ensure consistency across repositories. Use shared configuration files and tools to enforce standards.

4. **Scalability**:
   - **Monorepo**: Can handle a large number of services but requires proper tooling and practices to manage complexity.
   - **Polyrepo**: Naturally scalable as each service is independent. Easier to manage as the number of services grows.

### Conclusion

If you prioritize centralized management, consistency, and easier code sharing, a **monorepo** with proper tooling is a good choice. However, if you need more flexibility, autonomy for teams, and independent deployment, a **polyrepo** approach with shared libraries might be more suitable.

Ultimately, the decision depends on your specific needs, team structure, and long-term goals. Both approaches have their pros and cons, and the right choice will depend on how you balance these factors.


### decision al 10-10-2024

1. mantener tb boiler plate 
2. codear los microservicios de manera separada 
3. encontrar un mecanismo en el futuro para gestionar librerias y shared components