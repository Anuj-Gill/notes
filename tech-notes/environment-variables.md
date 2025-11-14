#tech-notes 

#### What really is process.env?
- it is a global object in node that contains environment variables
- by default, it only contains system env variables like PATH, USERNAME etc
- .env files are not automatically loaded, that where comes the role of packages like dotenv

#### What's dotenv?
- a lightweight package that reads .env and loads them into process.env
- it parses the .env file line by line and adds variables to process.env
```
// Before dotenv.config()
console.log(process.env.XYZ_VARIABLE); // undefined

// Load .env file
import * as dotenv from 'dotenv';
dotenv.config(); // Reads .env file and populates process.env

// After dotenv.config()
console.log(process.env.XYZ_VARIABLE); // "https://..." ✅
```

#### What does dotenv.config() do??
- Looks for a .env file in the project root
- reads each line, splits key value by = sign
- Adds to process.env