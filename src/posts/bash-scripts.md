---
title: 6 Terminal/Bash aliases, functions and scripts I use to 10x my productivity
slug: bash-scripts
excerpt: A terminal is a developer's best friend. A few keystrokes allow you to perform a swathe of actions such as creating hundreds of files and folders that would otherwise be cumbersome on a GUI...
date: 2023-06-08
image_link: https://cdn-images-1.medium.com/max/800/1*eSGNRPjuXtF7FRhUf-P-eg.jpeg
author: Bob Oyier
---

A terminal is a developer's best friend. A few keystrokes allow you to perform a swathe of actions such as creating hundreds of files and folders that would otherwise be cumbersome on a GUI.

Consider this code:

```bash
mkdir folder_{1..50}
```

It would create 50 folders at once named `folder_1` all through to `folder_50`.

```bash
touch file_{a..z}
# Creates 26 files named file_a through to file_z
```

Now, say, you are a photographer and would like to organize your photos from the last four years according to the months taken. You could manually create 48 folders named `2018-01` for January 2018 photos, `2018–02` for February 2018 photos… then repeat this process 46 more times until you have a `2022–12` folder for December 2022 photos.

However, if this photographer a little bit of Bash, they would have simply done this instead:

```bash
mkdir {2018..2022}-{01..12}
```

Are you not entertained?

![](https://cdn-images-1.medium.com/max/800/1*N2qJmjzdptgm2x8JcAAvaw.gif)

Cool, right! This is an example of what we call Brace Expansion (you can look it up, it's not the point of this blog).

Sometimes, though, some terminal workflows require repetitive actions which not only get tedious but pretty monotonous after a while.

You have setups that require you to run some commands first, then manually create a file or two, write a few lines of code inside those files, and then run some more commands (hello, Tailwind CSS)…

JUST… SO… TEDIOUS 😪😪!!

Luckily, Bash aliases, functions and scripts exist and they allow us to automate terminal processes to improve our productivity.

Bash scripts are a series of commands written inside an executable text file. Because they are executable, the files do not need an extension. But if you really must use an extension, use a `.sh` (shell) extension.

Whenever you call that file, all the commands written inside the script are run/executed top-to-bottom (unless you have a control-flow within it).

This means you have to be very mindful of the order in which you write the commands inside a script. Think of the chronological order of the actions you want to be automated.

Bash aliases on the other hand are written inside a `.bashrc` file. Just like its meaning in plain English, an alias is simply a nickname for an existing command. You could give, say, the clear command the alias `goodbye` and any time you type `goodbye` on your terminal, it will automatically know you intend to run the command `clear`.

To try an alias temporarily, run this command in your terminal:

`alias goodbye="clear"`

Now run the command go on your terminal. It does the same thing the clear command does.

Where there is too much logic in your automation, it's best to use a Bash script while simpler logic can be written as an alias or a Bash function.

A function is merely a Bash alias that can take an argument or arguments. Aliases by themselves never take any arguments.

Below are some of the aliases, functions and scripts I use in my environment to improve my productivity:

## 1. Committing code on GitHub.

One of my first gripes with the git workflow after learning it was having to `git add`, `git commit` and `git push` every single time I needed to push my code on GitHub. What if I wrote a script that could do that with command?

```bash
#!/bin/bash

git add .

git status

echo "Type your commit message in the text editor, Bob"

git commit

git push

git status
```

When executed, this script first adds all your unstaged code to the staging area, opens your preferred editor and waits for you to type your commit message. Once done typing your commit message and exiting your editor, the script commits the code and then pushes it to your GitHub repository.

Smooth.

I added a git status checker before commit and after commit in this script just to see confirm the cleanliness of the working tree

## 2. Cloning a repository

Whenever you clone a GitHub repository, you naturally want to `cd` into it, open it in an editor and start coding.

So why exactly do we need to do those last two steps manually every damn time?

```bash
function clone { git clone "$1" && code "$(basename $1 .git)"; }
export -f clone
```

This Bash function takes only one argument (that's what "$1" in the function means).

That argument is the link to the repository you want to clone on your terminal.

When I run the command, it clones the repository and opens it in VS Code (the editor I use. If you use a different editor, replace code in the function with your editor's name, say vi emacs or nano).

I don't need it to cd into the directory on my terminal because the VS Code instance it will open will have its own integrated terminal.

How to use it?
On the terminal, run the command like this (as an example):

```bash
clone https://github.com/oyieroyier/typescript-brad-traversy.git
```

## 3. Navigate quickly to a Directory

Sometimes the downside to being an organized developer is that a directory you are working on in a certain project may be nested too deep.

Say in your home directory you have a development directory inside which there is a frontend and a backend directory. Inside the frontend directory lies three directories named frontend_1, frontend_2 and frontend_3. You want a directory inside frontend_2 called react_projects inside which are 6 other directories.

Here is the file path for visualization:
![](https://cdn-images-1.medium.com/max/800/1*y5flAhZxClrhmTS7YIo1og.png)

An amateur developer may need to memorize the exact path of the directory each time he needs to work on a React project. Or navigate to it step by step.

However, a Bash script like the one below will enable you to get to your nested directory with just one command.

```bash
#!/bin/bash

cd ~/development/frontend/frontend_2/react_projects/ && ls
```

Not only does executing this script take you directly to the react_projects directory, it additionally lists all the files and directories available inside the last directory on the path so you can see which one you want to work on.

To execute this script, I have an executable file in my home directory because that's where my terminal always opens on launch.

If the file is named `react`, I simply run the command (note the space between the fullstop and the filename):

```bash
. react
```

## 4. Creating a React App with Create React App

My scripts are usually inspired by what I call "the next natural step" logic.

It's simply a matter of asking myself, what do I naturally find myself doing after performing a certain action? And then the follow-up question to that would be, "Can that next natural step be automated together with the first step?"

I found that any time I created a React application, my next natural steps, in order, were to:

1. `cd` into that app
2. Open the app in a text editor/IDE/
3. Run the local server
4. Perform more setup actions like installing React Router Dom or React Icons

Create React App is on its deathbed at the time of writing this but before I moved to Vite, I wrote this script to set up all my React projects using CRA:

```bash
echo "What's the name of the React project?"

read project_name

npx create-react-app $project_name

cd $project_name

mkdir ./src/scss

touch ./src/scss/index.scss

npm i react-icons --save-prod

npm install react-router-dom@6.4

touch .env

echo "PORT=4200" > .env

code .

npm start
```

The `read` command in Bash is like the `input` command in other languages like Python and prompt in JavaScript.

That means the value the user enters will be saved in the variable `$project_name` and be reused in that script.

After entering the project_name and hitting Enter, CRA will:

1. Create a project with that name.
2. `cd` into that project
3. Create an `scss` directory with an `index.scss` file in it.
4. Install React Icons.
5. Install React Router DOM.
6. Create a `.env` file and set my React App to run on port 4200 instead of 3000.
7. Open the project on VS Code so I can start working on it.
   And finally, it runs the local server.

All by running one command. Awesome, right?

####Bonus:

Since I no longer use CRA, this is the Bash function I replaced it with for creating a Vite app:

```bash
function vite { npm create vite@latest "$1" --template react && cd "$(basename $1)" && npm install && npm i react-icons --save-prod && npm install react-router-dom && code . && npm run dev; }
export -f vite
```

It's a bit more minimalist but I always prefer my projects to start with React Icons and React Router Dom installed.

When called (with strictly one argument), the function will:

1. Create a Vite App with that name.
2. `cd` into that project directory.
3. Install all default dependencies.
4. Install React Icons.
5. Install the latest available version of React Router DOM.
6. Open the project in VS Code.
7. Run the local server.

To create a project called soccer, I would only run the command:

```bash
vite soccer
```

## 5. Tailwind CSS setup

Initializing a Tailwind environment and building input/output files can be a pain in the neck.
I wrote this alias to set up everything I need for any initial Tailwind project environment (non-React. I use a different one for React):

```bash
alias tail="npm init -y && npm i -D tailwindcss && npx tailwindcss init && touch input.css index.html style.css && echo -e '@tailwind base;\n@tailwind components;\n@tailwind utilities;' > input.css && npx tailwindcss -i ./input.css -o ./style.css"
```

The command above does the following:

1. Initiates an NPM project. The `-y` flag means "Choose 'Yes' for every option.
2. Installs Tailwind CSS via npm.
3. Creates a `tailwind.config.js` file.
4. Creates an `input.css` file which acts as the main CSS file.
5. Creates an `index.html` file.
6. Creates a `style.css` file which acts as the output file.
7. Adds the `@tailwind` directives for each of Tailwind's layers in the main CSS file.
8. Starts the Tailwind CLI build process.

## 6. Setting up a Rails project.

Setting up a Ruby on Rails project also comes with overwhelming optional commands and flags that determine the shape your Rails app will take.

For my Rails projects, I prefer to manually set up git because there are times I want the Rails API inside a monolith git repository.

I also prefer to initialize all my projects with Active Model Serializers from the get-go (which means any time I run rails g resource, it automatically creates serializers too. This is not the default behavior of Rails).

I absolutely love the Faker Gem for data seeding and also start all my projects with it installed. Lastly, I skip JavaScript when installing Rails.

The below script does all that for me.

```bash
#!/bin/bash

echo "Enter name of your Rails project."
echo -e "Follow the Rails naming conventions:\n\t1. A short but decriptive project name.\n\t2. Project name must be in all lowercase.\n\t3. No underscores. Use hyphens to separate multi-word project names."

read project_name

rails new $project_name --skip-javascript --skip-git

cd $project_name

bundle add active_model_serializers

bundle add faker

code .

rails s
```

For this script, I also added a neat paragraph which reminds me to follow the Rails naming conventions any time I create a project.

Context switching can sometimes bring unexpected and undesirable effects:
![](https://cdn-images-1.medium.com/max/800/1*oxig5fYKtDN09Rr2-G3oXQ.jpeg)

After entering a proper project name, the script will set it up using my chosen flags and add the AMS and Faker in my Gemfile then open the project in VS Code for the real work to begin. Meanwhile, in the background, it starts a Puma Rails server. Awesome!

### Conclusion

As you have seen, Bash aliases, scripts and functions are powerful tools that you can leverage to make coding fun and take your productivity to the next level by eliminating a chunk of redundant tasks.

I hope you enjoyed this article and will leverage the power of Bash scripts to take your productivity to the next level
.
