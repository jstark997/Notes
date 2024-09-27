# Start A New App

## Prerequisites

- **Node.js**: Ensure you have Node.js installed. You can download it from [Node.js official website](https://nodejs.org/).

## Step-by-Step Guide

### 1. Create a New Directory for Your Project

Open your terminal and navigate to the directory where you want to create your new Next.js project.

### 2. Run the Create Next App Command

Execute the following command to create a new Next.js project. This will set up everything you need to get started.

```sh
npx create-next-app@latest
```

You may be asked if it is ok to install the latest version of Next.js

```sh
Need to install the following packages:
create-next-app@14.2.3
Ok to proceed? (y)
```

Answer 'y' to proceed.

During the project set up process you will be asked to answer the following questions:

```sh
√ What is your project named? ... my-test-app
√ Would you like to use TypeScript? ... No / Yes
√ Would you like to use ESLint? ... No / Yes
√ Would you like to use Tailwind CSS? ... No / Yes
√ Would you like to use `src/` directory? ... No / Yes
√ Would you like to use App Router? (recommended) ... No / Yes
? Would you like to customize the default import alias (@/*)? » No / Yes

```

| Question                                                     | Default | Purpose                                                                                    |
| ------------------------------------------------------------ | ------- | ------------------------------------------------------------------------------------------ |
| What is your project named?                                  | my-app  | A new directory will be created with this name and the project will be placed there.       |
| Would you like to use TypeScript?                            | Yes     | Adds support for TypeScript. 'Yes' is recommended.                                         |
| Would you like to use ESLint?                                | Yes     | Adds ESLint for automatic code quality checking. 'Yes' is recommended.                     |
| Would you like to use Tailwind CSS?                          | Yes     | Adss Tailwind CSS support to provide some default styling.                                 |
| Would you like to use `src/` directory?                      | No      | Answering 'Yes' will place all code inside a folder called 'src'.                          |
| Would you like to use App Router?                            | Yes     | Enables the use of AppRouter for page routing. 'Yes' is highly recommended.                |
| Would you like to customize the default import alias (@/\*)? | No      | Answering 'Yes' allows importing to be absolute rather than relative. 'No' is recommended. |

### 3. Navigate to Your Project Directory

After the project is created, navigate into the project directory:

```sh
cd my-next-app
```

### 4. Start the Development Server

To start the development server, run:

```sh
npm run dev
> my-test-app@0.1.0 dev
> next dev

  ▲ Next.js 14.2.3
  - Local:        http://localhost:3000

 ✓ Starting...
 ✓ Ready in 13.7s
```

### 5. Stop the Development Server

To stop the development server enter Ctrl-C and answer 'y' to terminate the batch job:

```sh
Terminate batch job (Y/N)? y
```
