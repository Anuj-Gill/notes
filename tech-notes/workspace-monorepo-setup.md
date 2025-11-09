#tech-notes 

1. setup package.json in the root folder. ex:
```
{
  "name": "meme-stock-exchange",
  "version": "1.0.0",
  "private": true,
  "workspaces": [
  "backend",
  "broker"
  ],
  "repository": "https://github.com/Anuj-Gill/meme-stock-exchange.git",
  "scripts": {
  "broker:dev": "yarn workspace @meme-stock-exchange/broker dev"
  }
} 
```

2. **Create individual projects** (e.g., `backend`, `broker`, `ui`, etc.)  
   Add each project’s folder path inside the `workspaces` array in the root `package.json`.

3. **Naming conventions for sub-projects**  
   In each sub-project’s `package.json`, follow the scoped naming convention:
```
{   "name": "@meme-stock-exchange/backend" }
```
    This helps uniquely identify packages across the monorepo.

4. **Cross-project imports**  
   When using code from another workspace, import it using the same scoped name:

```
import { someFn } from "@meme-stock-exchange/utils"
```
	This keeps imports consistent and makes shared utilities easier      to manage.