## Setup PostgreSQL With TypeORM

## What is an ORM ?
ORM Stands For Object Relational Mapping Used To Convert Data Between Incompatible Type System Using Object Oriented Programming Languages Like JavaScript, TypeScript, Python
## Popular ORM's
* JavaScript
    - Sequelize
    - Prisma
* TypeScript
    - TypeORM
    - Mikro-ORM
* Python
    - SQLAlchemy
    - Django ORM

## Why TypeORM
If You Know Me, I Am a Big Fan Of TypeScript And I Like Both Mikro-ORM And TypeORM But The Thing With TypeORM That It Is More Of Like TypeScript Syntax. Basically I Like TypeORM Syntax Than Mikro-ORM

## Setting Up TypeORM
* Requirements
    - [Node.js](https://nodejs.org)
    - [NPM](https://npmjs.com)

### Creating A Basic Node.js App
```bash
# NPM
npm init
# Yarn
yarn init
```
### Adding TypeScript
```bash
# NPM
npm install -D typescript @types/node nodemon
# Yarn
yarn add -D typescript @types/node nodemon
```
* Dependencies
    - typescript - Compiler For TypeScript
    - @types/node - Types For Node.js
    - nodemon - Restarts The Server On Changes
    
Open Your package.json And Add Scripts To It
```json
"scripts": {
    "start": "node dist/index.js",
    "dev": "nodemon dist/index.js",
    "watch": "tsc -w"
}
```
Now We Need A tsconfig.json, So Instead Of Typing tsconfig.json From Scratch Let's Generate It
```bash
npx tsconfig.json
```
Select Node.js When Asked To Enter Your Framework

### Run The Application
Create a File `src/index.ts` And Add This Code To It
```ts
console.log('hello world')
```
Now Open a Terminal And Run watch Command
```bash
# NPM
npm run watch
# Yarn
yarn watch
```
Open Another Terminal And Run dev Command
```bash
# NPM
npm run dev
# Yarn
yarn dev
```
You Should See **hello world**
### Add TypeORM 
```bash
# NPM
npm install typeorm pg reflect-metadata
# Yarn
yarn add typeorm pg reflect-metadata
```
* Dependencies
    - typeorm - Base TypeORM Package
    - pg - Postgres Driver For Node.js
    - reflect-metadata - Required For TypeORM

Now We Can Create A Connection To Our Database
Open `src/index.ts` and add async connection to typeorm
```ts
import "reflect-metadata"
const main = async () => {
    console.log('hello world')
}
main().catch(err => console.error(err))
```
Now we can use async/await syntax inside main function
we can setting up typeorm connection using the `createConnection` function

```ts
// src/index.ts
import "reflect-metadata"
import { createConnection } from "typeorm"
const main = async () => {
    await createConnection({
        type: "postgres", // DATABASE TYPE
        host: "localhost", // DATABASE HOST
        port: 5432, // DATABASE PORT (DEFAULT FOR POSTGRES)
        username: "test", // USERNAME
        password: "test", // PASSWORD
        database: "test", // DATABASE
        logging: true, // LOG THE GENERATED SQL
        synchronize: true // FALSE ON PRODUCTION
    })
}
main().catch(err => console.error(err))
```

## Creating An Entity (Table)

```ts
// src/entities/User.ts
import { Entity, PrimaryGeneratedColumn, Column, BaseEntity } from "typeorm";
@Entity()
export class User extends BaseEntity {
    @PrimaryGeneratedColumn()
    id: string;
    @Column()
    firstName: string;
    @Column()
    lastName: string;
    @Column()
    isActive: boolean;
}
```

Now if you open your terminal running the `dev` command you should see generated SQL

# CRUD
## CREATE
```ts
const user = new User({firstName: "Nouman", lastName: "Rahman", isActive: true});
await User.save(user)
```
## READ
```ts
await User.find() // Give All Users
await User.findOne("1") // Give The User With ID ("1")
```
## UPDATE
```ts
await User.update({id: 1}, {firstName: "Nouman"}) // Update firstName Of The User With ID ("1")
```
## DELETE
```ts
await User.delete({id: 1}) // Delete The User With ID ("1")
```