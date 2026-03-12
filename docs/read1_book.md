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

# you can check the verson of Typescript using
tsc -v

npm init -y

npm i express
npm i multer   # Used for file upload 
npm i dotenv
npm i -D nodemon typescript @types/node  ts-node
```

Run the command `npm run build`, To build our project.


----



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

## Install langchain and langgraph


`ChatTogetherAI` it is API offers 50+ open source models
langchain itself mention that : https://docs.langchain.com/oss/javascript/integrations/llms/together  (Now paid)

Too see all proders : https://docs.langchain.com/oss/javascript/integrations/providers/all_providers


> Firework
- tempmail: woceli3554@dnsclick.com
- password: salatuvalAtuboy1234@3456

(Firework no longer offer completely free LLM models, It charges some amount at lest $0.1 )  

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


> NOTE:
- `ts-node` Run the typescript using CommonJS. works when your project does **NOT use "type":"module"**. Use `requires()` internally.
- `ts-node-esm` : Run Typescript using ES Modules (ESM). Needed when your **package.json has "type":"module"**. Uses `import/export`

<br>

`tsx` is not downloaded by default with the installation of tsc.

tsc is TypeScript Compiler. It converts .ts --> .js . Does full type-checking. Create output file. Used for production. Used to build project. 

tsx i TypeScript eXecutor(runner). Run .ts file. No separate build step [no any intermidiatory .js file is created]. Faster for scripts. Uses lightweight taspilation(esbuild). It is used to Run the file, Faster than tsc

<br>

The `ts-node` and `tsx` Both do the same work. Run typescript files directly without creating a `.js`.
- ts-node is old tool, heavy memory usage, slow.
- `tsx` is new tool, very fast, lightweight. 

> This is how tsx win the battle
- It is faster than ts-node
- excellent support with tsconfig.json
- memory usage ~35 mb, whereas ts-node use ~125 mb
- installation size is also less compared to ts-node
- beginner friendly. zero configuration required just install and start using it. 



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




visit: index2.ts

visit: index3.ts


The process of **writing refining and optimizing inputs** to encourage Generative AI to create a a specific high-quality outputs.

1. Zero shots ---> **Don't provide example** to AI model how it should perform the task
```js
{
  role: "system",
  content: "You are a helpful assistant that translates English to Hindi. Translate the user sentence.",
}
```

2. Few shots ---> **Guide** the model how to perform the Task by providing few exmples. For what input what output you expects.
```js
{
  role: "system",
  content: `You are a helpful assistant that translates English to Hindi. Translate the user sentence.
  
  eg. How are you ? : केसे हो तुम ?
  
  `,
},
```
3. chain of thoughts (COT) --> COT is prompt engineering technique that tell AI to think step by step. 



----------------------------------------------------------


## We have different types of messages
- SystemMessage : SystemMessage is like we give the AI a specific role.
- AIMessage
- HumanMessage : A message(question,input) provided by human




## Few example of TypeScript configuration files, So that you can understand the Typescript configuration files.
Typescript configuration file examples

visit : ./typescript_example.md

### ESM(ES Module) vs CommonJS


ESM
- Asynchronous (parallel fetching style)
- "type": "module" , ".mjs" , and In Typescript "module": "ESNext"
- Before code run plan how the execution is going to perform. Parse(see) all module -> execute them in their dependency order
- Main thread is non-blocking usually

Parse all `import` statement. Build dependency graph. rewire everything. Start execution from the entry point(eg. index.js or main.jsx) , but actually evalute deepest dependency first [like post-order traversal]

<br>

CommonJS
- Synchrounous (blocking nature)
- "type": "commonjs" , ".cjs" files
- When it see`require()` it immediately load and execute
- Block the Main thread until the module is fully loaded and executed. Block untill the module is fully loaded & execute.

immeidately(sunchronous) way read the file. execute it top-to-bottom. If it has `require()` call it. (recursively in sub-files also, blocking nature.)

<br>

Both ESM and CommonJS(CJS) execute top-to-bottom. But the difference is when and how modules are loaded and initialiezed, not the execution order inside a file.

### The difference between `import "dotenv/config";` vs `import dotenv from "dotenv";` 

1. `import "dotenv/config";`
It Automatically calls `dotenv.config()` for you at import time. One liner, clean, no extra setup
- It assumes the `.env` is present in current directiory where your `.js` file is present [ inside which you use the import statement ] .
- OR you can set an environment variable and run the `.js` file `DOTENV_CONFIG_PATH=/custom/path/.env node app.js`  



2. `import dotenv from "dotenv";`
- `dotenv.config({ path: './custom/.env', debug: true });`
- Default import gives you `dotenv` object. You need to call the **dotenv.config()** method.
- Allows customization (custom path, encoding, debu, override, etc.)

---

OR use the CLI **--env-file** flag `node --env-file=./custom/.env app.js`. This is node.js feature not **dotenv**

## Ueful commands 

```bash
npm --> npx
pnpm --> pnpx (older) ->`pnpm dlx tsx index3_3_groq.ts` (modern)
```

```
git commit -a --amend --no-edit
git commit -a --amend --no-edit
```

# Chapter 3: Tool Calling

Tool allow the Agent to perform an action.

If we ask the current weather in New Yourk. The LLM has cutoff time the agent is not trained on latest data(Today's weather). OR if we ask it to generate chart about current population(latest data) in miami. Then still it can't anwer. 

But if you ask what is value of x if x + y = 12 then it will say "12-y". The models are trained on some mathematical book, That contains some basic arithmatic operations, equation solving, tables, etc.. So it is able to solve this .


That is where we give a tool to the Agent in order to perform an action.

**Agent  + [ search_tool, chart_generator, json_convertor ]**

now we can ask questions like 
- current weahter
- chart about population
- math problem

before understanding tool, Understand `Runnable`

### Runnable
Runnable is way of taking the output of function and pass it as input to another Runnable. 
- Runnable has some common methods already implemented `.invoke(), .batch(), .stream()`


Runnable1 --> Runnable2 --> Runnable3


### more

Q. The newer version of dotenv prints the log message of loading `.env` each time when you run the program `[dotenv@17.3.1] injecting env (4) from ../.env -- tip: ⚙️  write to custom object with { processEnv: myObject }` to remove it 

Solution: `dotenv.config({ path: '../.env',debug: false, quiet: true});`


Q. After migrating to the pnpm there is big command i need to write to run the code `pnpm dlx tsx index.ts`

Solution:

- `dlx`  standas for DownLoad and eXecute. Download package temporarily, run it, and then discard it. You don't need to unstall the package permanenetly in your project or globally
- `pnpx` and `pnpm dlx` both are almost same idea interally "pnpx" calls "pnpm dlx". so, **pnpx = pnpm dlx**

> It is just recommendation to use `pnpm dlx` because it gives more readability. However both internally does same thing

<br>

1. Add package.json command , But then it will no longer be dynamic
2. install tsx locally (much faster because it doesn't downloda each time)
```bash
pnpm add -D tsx
pnpm tsx index.ts
```

3. install tsx globally
```bash
pnpm add -g tsx
tsx index.ts
```

4. Create a shell alias
```bash
alias tsrun="pnpm dlx tsx"
tsrun index.ts
```


**My Recommedation**
> pnpm add -D tsx + set:  `alias run="pnpm tsx"`

problem: But with "pnpm tsx" you need to mention the path relative to the package.json



**More easy**<br>
```bash
pnpm add -g tsx

#If it gives :  ERR_PNPM_NO_GLOBAL_BIN_DIR  Unable to find the global bin directory

#then run : 
pnpm setup
source /home/codespace/.bashrc
```

> Now you can also run this way `tsx index.ts`. OR if you want to make it more reliable then, add **shabang** at top

```js
#!/usr/bin/env tsx

console.log("Hello from TypeScript 🚀")
```

```bash
chmod +x index.ts

# NOTE: if the file doesn't have execute permission then even after you hit the tab ( ./inde). A linux compatiable machine will not do autocompletion.

./index.ts
```

very clean

---

### something cool  `ask "What is langchain?"`

The trick can be done using a command runner so your script behave like real CLI commands. A popular tool used in many dev / AI repos is `just`

Just is an open-source software toolkit for automating and managing build and test workflows, developed by Microsoft. 

just was intially created for javascript/Typescript but now it is general-purpose command line runner and work with almost every programming language. 

1. Install just 
```bash
sudo apt install just
```

2. create a `justfile`
```bash
project/
 ├ package.json
 ├ justfile   <---------- Here is the file
 └ scripts/
      ask.ts
      rag.ts
      embed.ts
```

Example:
```bash
ask question:
    tsx scripts/ask.ts {{question}}

rag file:
    tsx scripts/rag.ts {{file}}

embed file:
    tsx scripts/embed.ts {{file}}
```

Run the command:
```bash
just ask "What is LangChain?"
```

> MK: It is not like that you have to put the command line input. If you use it to just reduce your large command-line command then still ok 
>> This is something MK is finding since a long time. 
> This works on Windows🪟 also 

For that you need to modify code so that LangChain can get the question from the command line


```js
// ask.ts

const question = process.argv[2];
console.log("Question:", question);
```

```bash
just ask "What is RAG?"
# output: Question: What is RAG?
```


Explanation
```text
process.argv
[
  node_path,
  script_path,
  arg1,
  arg2,
  ...
]

```


## TavilySearch

install tavily

and set the api key in .env file

---


command to remove the cache from the git repo. Previously tracking some files but now don't want to track
If Git was previously tracking files but you added them to .gitignore and want Git to stop tracking them, you need to remove them from the Git index (cache) while keeping them in your local filesystem.
`git rm --cached path/to/file`