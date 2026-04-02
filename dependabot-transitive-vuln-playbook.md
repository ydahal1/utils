# Fix Vulnerable npm Packages

## 1. Find vulnerabilities
npm audit

## 2. Find what depends on the vulnerable package
npm why <package-name>  

## 2.1 Now look at the dependency tree 
pnpm info <socket.io-client@4.1.3> dependencies

## 3. Check available versions of the parent package
npm view socket.io-client versions

## 4. Check if the latest non-breaking version removes the vulnerable dependency
pnpm info socket.io-client@4.1.3 dependencies
- **If fixed:**  
  pnpm add -D <package-name>  
  npm install

- **If not fixed:**  
  Check breaking versions. If a newer version removes the vulnerable dependency, upgrade and run:  
  npm install

- **If all versions still use the vulnerable package:**  
  Add an **override** in `package.json` and run:  
  npm install
