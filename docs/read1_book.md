# Overview

From left panel you can select sources
- google drive 
- link : website , cyoutube
- pasted text


Chat section
- It has memory. Remember previous questions(Remember your name)


From right panel. We can generate
- summary
- guide
- brief in doc
- faq
- audio
- mind map


Paymane integration


Authentication we use Google

---------------------------------------------------


## Prequsites

- React 
- Node/ Express Basics


- Vector databases
- Embedding models 
- RAG(Retrival Augamented Generation) from scratch
- Lanchain 
- Payment with stripe
- Background job
- Google Auth
- AI Agent with memory
- AI Agent that uses Tools 
- Prompt engineering


## Tech Stack

MERN Stack
- MongoDB
- Expressjs
- React
- Nodejs

Designing
- shadcn UI
- Tailwind CSS



Model providers
- ChatFireWork( no longer free Paid)
- ChatCerebra(free)

- ChatTogether (paid)


--------------------------------------------------------------

# Chapter 1 : Setup and Start Project 


## Run the commands


Node version : 22.x

```bash  
npm i -g typescript             : Install typescript globally


npm init -y

npm i express
npm i multer   : Used for file upload 
npm i dotenv
npm i -D nodemon typescript @types/node  ts-node
```


`-D` is alias for `--save-dev`: It will add your package inside the `devDependencies` section of your `package.json` file
- Packages Installed this way were instended only for local developmenent and testing (like Nodemon) and will not included when application deployed to producion.

------------------------------------------------------------------------

- `@type/node`: It is package that provide type defination for buit-in node.js API like `fs`, `path`, `http`. Since node.js is written in Javascript. Typescript doesn't know what a fucntion expect or return. 
    - That package tells typescript things like `process.env` , `__dirname` exist so you don't get "cannot find name" errors

    - The `@` and `/` indicate **scoped package** that acts as a namespace to group related packages together.
        - @scope-name/package-name :  The scope name aka symbol. It is uniqe in the world of npm. Generally scope name = publisher name (in your case "mayank")
        - **Namespace** is like a folder or last name(same family). A containt that keep names organized; so that they don't crash into each other. The same analogy: One folder can not have two files with same name. Without namespace only one person in the world of npm could own the name `utils`. With namesapce(scope) thousands of peoeple can have their own `utils`. (google's utils, microsoft's utils, Your own utils, etc...)

----------------------------------------------------------------------------

- `ts-node`(Execution engine) : Typescript execution engine allows you to run Typescript files directly without manually compiling the to javascript first. 
    - It "hooks"(fit) into the node.js and trasforms Typescript code into javascript on the fly(JIT) right before execution.    
        - **hook means** jump into the middle of node and act as traslator. Everytime node tries to read `.ts` file `ts-node` quickly traslate it to into Javascript on the fly and feed that javascript back to Node.js 

        ```js
        ts-node app.ts
        ```


        - The Node.js version 22.6+. Now has built-in traslator using method called **type stripping** This means you don't need to install extra `ts-node` tool anymore. Node can just strip away the typescript parts itsefd and run the code as normal javascript.

        ```js
        node --experimental-strip-types hello.ts
        ```
        > What Node does: It sees colon(:) string and removes it, then runs the remaining JavaScript.
        
        In, Node 23.6.0+
        This feature enbaled by deafault
        ```js
        node hello.ts
        ```



    - instead of running `node server.js` you can run `npx ts-nodeserver.ts`
    - It is primarily used for development and testing. For production it is still recommeded to compile your code into standard javascript using `tsc` for better performance. 


Here since yu gave not installed `ts-node` globally. and you installed it as development dependency `npm install --save-dev ts-node`. You can not run the **ts-node index.ts** directly in the command line. 
By default the command line tries to find it in gloabl `node_modules/` folder Either you should download
- Either you should download it globally OR `npx ts-node index.ts` OR `npm run <script-name>`. sciprt-name is mentioned inside the package.json, it can access local workspace's *node_modules/.bin*
- When you run `npx ts-node index.ts`. First it look in local workspace `./node_modules/.bin`, if not found -> check global `./node_modules/.bin`. If still not found download the temporary copy and store it in npm cache.

Usually, npm cache location:
- Linux: `~/.npm/_npx/`
- Windows: `C:\Users\<your-username>\AppData\Local\npm-cache\`

You can get the exact path using: `npm config get cache`



> Node js is runtime of javascript , means Nodei s is the environment where we can run the javascript code outside the browser. 


`tsc`(TypeScript Compiler): It compiles typescript to javascript file. Creates `.js` files on disk. You run the outout JS files with node.js. *Used for production builds*.

`ts-node` compiles and run TypeScript directly. No `.js` files creates (compile in memory - RAM). Like running a `.ts` file instatntly. *Used for development*.


> visit:  src/index.ts

-------------------------------------------------------------------------------------------

Then install langchain and langgraph


`ChatTogetherAI` it is API offers 50+ open source models
langchain itself mention that : https://docs.langchain.com/oss/javascript/integrations/llms/together  (Now paid)

Too see all proders : https://docs.langchain.com/oss/javascript/integrations/providers/all_providers


> Firework
- tempmail: woceli3554@dnsclick.com
- password: salatuvalAtuboy1234@3456

profile > settings > api_key

----------


`npm install @langchain/community --legacy-peer-deps`

- **--legacy-peer-deps** is flag you pass to `npm install` to tell to bahave more like old npm v4-v6 did when handling peer dependencies

    - npm 6 and earlier : peer dependency give just warnings. npm install anyway even if versions don't match.
    - npm7 and 7+ peer deps becomes errors by default. npm refuse to install if any dependency conflict exist. 
    - In fast-moving ecosystem like LangChain, React, Next.js, etc. They using narrow version ranges from 1-2 years 
    - so `--legecy-peer-deps` basically says: "Please go back to the old, more forgving behaviour: treat peer dependency mismatches as warnings instead of hard erros, and let me install anyway"

-----------------------------------

- `dependencies`: packages your code needs to run (production time)
- `devDependencies`: packages only needed to run development (testing, linting, building, etcl)
- `peerDepedency`: Package(eg. React) your library(eg. shadcn) expects the consumer(your app) already provide the exact/similar version.
    - This way npm won't install React again when you install shadcn - It expects the app already has React 18. this prevents duplicate React copies. 
    - shadcn says: "hey consumer(you React app) you need to give me this exact/simiar version of React - don't make me bundle by installing React's another copy along with me!". That it says by mentionin react inside the peer dependencies.

        ```js
        {
        "peerDependencies": {
            "react": "^18.0.0",
            "react-dom": "^18.0.0"
        },
        "devDependencies": {
            "react": "^18.2.0",          // ← you still need React to develop & test your components locally
            "@types/react": "^18.2.0",
            "vite": "^5.0.0",
            "typescript": "^5.0.0"
        }
        }
        ```

------------------------------------------

`overrides` in package.json. overrides gives you control over dependency tree for any dependency or trasitive dependency. When automatic resolution pick a wrong version. 
1. npm install builds full dependency tree
2. it normally respect what each package declared in their **peerDependencies** or **devDependencies** 
3. When npm sees overrides entry in your `root package.json` , It rewrite the dependency tree with maching package everywhere in the tree.

```json
// Force a single package to a fixed version everywhere
{
  "overrides": {
    "dotenv": "^17.2.3"
  }
}
// → Every time any package in your tree asks for dotenv (direct or nested), npm installs ^17.2.3 instead.



// more Powerful
// override only when a certain parent package "@browserbasehq/stagehand"" depends on dotenv
{
  "overrides": {
    "@browserbasehq/stagehand": {
      "dotenv": "^17.2.3"
    }
  }
}
// → When resolving dependencies of @browserbasehq/stagehand, force dotenv to ^17.2.3.
// → Other packages that ask for dotenv keep their original requested version.

// Simple explantioin simple way one statement
// @browserbasehq/stagehand depends on dotenv v16 but npm force it to use dotenv v17 
// because most of ther packages uses v17 or v17 is already installed

```

`overrides` only works in root's package.json

After adding changes always run
```bash
rm -rf node_modules package-lock.json
npm install
```

so that newly required version's packages can be installed.



-----------------------------------------------------------

`tsx` is command line tool CLI for running typescript directly from Node.js. Without need to compile it to javascript first. (like you do with tsc)

tsx is currently(2026) is one of the easiest, fastest, and most developer friendly way to run Typescript scripts and small servers direcly in node.js that's why it becomes goto recommenddation for many developers who usse `ts-node`

code example 
```bash

# tsx
npx tsx src/index2.ts

# ts-node
npx ts-node-esm src/index2.ts

```

How tsx win the battle
- It is faster than ts-node
- excellent support with tsconfig.json
- memory usage ~35 mb, whereas ts-node use ~125 mb
- installation size is also less compared to ts-node
- beginner friendly. zero configuration required just install and start using it. 


> NOTE:
- `ts-node` Run the typescript using CommonJS. works when your project does **NOT use "type":"module"**. Use `requires()` internally.
- `ts-node-esm` : Run Typescript using ES Modules (ESM). Needed when your **package.json has "type":"module"**. Uses `import/export`


-------------------------------------------------------

After `import 'dotenv/config';` what happen

1. dotenv reads your **.env** file
2. It parses the key-value pairs
3. It add them to `process.env`. process.env is **built-in global object in Node.js**

It does this so that, below functionality can works
```js
console.log(process.env.DATABASE_URL); // works
```

`process` is object that is has information about process(.js file running process). It has **property / key** `env` and it's value will be another JS object(key-vlaue pairs). That contians information basically your environment variable.

This is not a global variable that you declared by yourself 
```js
const MY_KEY = "f-ssdjladna....."
```
You can  not do `console.log(MY_KEY)`. You must write `process.env.MY_KEY`



In Widows Operating system, This thing `console.log(process.env)` when you run in Node.js script. It shows merged view that includes both : *System env variables*(for all users) + *User env variables*(for currently logged-in user)

It doesn't show only one of them, It shows both. It starting with all system variables, Then overwriting as per user variables(if repeated.)

For Windows OS, Node.js uses `;` to separate two paths, Whereas In Linux it uses ` 

> In Github Codespace type **lsb_release -a** to know what Linux distribution it is using.

-------------------------------

The popular Global variables  JS have and Node.js have

1. JS have (Browser / Client-side JavaScript)
- `window` : A global object
- `document` : DOM root
- `console` : logging/debugging
- `fetch` : HTTP request API
- `setTimeout` : scheduling function 
- `alert` / prompt / confirm : Classic popup dialogs
- `localStorage`/sessionStorage : Browser storage
- `navigator` : Browser & device infornation
- `location`: Current URL ( used to implement React functionality [Like functionality dynamically change the browser URL without reloading the page] from scratch )
- `history` : browser history
- Buit-in JS objects : Math, Date, JSON, Array, ... (same in Node.js)


> These all things comes from broswer, Not your JS .js file contains these functioinalities. 

In browser script( non-module `<script>` tag, type != module. It's CommonJS). declaring `var x = 10` or `x=10` will makes it property of `window` -> `window.x`


2. Node.js (server-side javascript)
In Node.js, the global object is called `global` or `globalThis` (modern).

The top-level `var / let / const` do NOT become global by default - they say module scoped. (This is biggest difference)

- process : Current Node.js process information & control (An Object)
- console : mostly identical to browser's console object
- __dirname : directiory of current file [A property]
- __filename : full path + file name of current file
- module: Current module information (ESM , CommonJS)
- require : commonJS module loader [Available globally] [-- A function]
- Buffer: Binarydata handling
- setTimeout / setInterval / setImmediate : Same as browser + setImmediate
- queueMicrotask : modern scheduling
- URL / `URLSearchParams` : URL parsing and manipulation


To create a real global variable in node.js you must do `global.myvar = 123;`


> NOTE:

Value of `this` in:
- commonJS : window object
- module : undefined


# Chapter 2 Prompt Enginnering


visit: index3.ts


The process of **writing refining and optimizing inputs** to encourage Generative AI to create a a specific high-quality outputs.

- Zero shots ---> **Don't provide example** to AI model how it should perform the task


```js
{
  role: "system",
  content: "You are a helpful assistant that translates English to Hindi. Translate the user sentence.",
},
- Few shots ---> **Guide** the model how to perform the Task by providing exmples
```
```js
{
  role: "system",
  content: `You are a helpful assistant that translates English to Hindi. Translate the user sentence.
  
  eg. How are you ? : केसे हो तुम ?
  
  `,
},
```
- chain of thoughts (COT) -----> tell the model to perform the job by walking **through the step**




----------------------------------------------------------


We have different types of messages
- SystemMessage
- AIMessage
- HumanMessage
