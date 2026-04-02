# Fix Vulnerable npm Packages

## 1. Find vulnerabilities
npm audit

## 2. Find what depends on the vulnerable package
npm why <package-name>  
npm ls <package-name>

## 3. Check available versions of the parent package
pnpm info <parent-package> version

## 4. Check if the latest non-breaking version removes the vulnerable dependency
npm ls <package-name>

- **If fixed:**  
  pnpm add -D <package-name>  
  npm install

- **If not fixed:**  
  Check breaking versions. If a newer version removes the vulnerable dependency, upgrade and run:  
  npm install

- **If all versions still use the vulnerable package:**  
  Add an **override** in `package.json` and run:  
  npm install
