1. **Dependency Management**
2. Global and Local Package Installation
3. **Script Management with `package.json`**
4. Version Control with `package-lock.json`
   - The `package-lock.json` file ensures that all installations result in the same file structure across different environments, avoiding "it works on my machine" issues.
   - This file locks down the versions of all installed packages and their dependencies, improving reproducibility.
5. Custom Configuration and Management
	- Configure npm settings like proxies, registries, or even custom scripts using the `.npmrc` file.
	- npm supports workspaces, allowing you to manage multiple packages within a single monorepo setup.
6. Security Features - Use `npm audit` to scan your dependencies for vulnerabilities and potential security risks
## NPX
