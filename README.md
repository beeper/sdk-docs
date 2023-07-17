# Beeper SDKs
This repo will contain instructions for various SDKs that can be used with Beeper.

# Widgets

Widgets are webpages that live on the sidebar of Beeper. They can access data from a chat in Beeper, such as messages and a list of users. They can also take actions, such as sending messages on the user's behalf. 

### Get started in 5 minutes

We'll set you up with a [NextJS](https://nextjs.org/) project that will provide a foundational codebase you can then build upon.

If you haven't already used NextJS, don't worry - you can just write React code, and NextJS is just a layer on top that handles things like routing, hot-reloading, server-side rendering, performance optimizations, and other things that make the developer experience nicer.

#### Install Node

If you haven't already, install the latest LTS or Current version of Node.js. It is recommended to also install `yarn` as well:

```bash
npm install --global yarn
```

#### Clone the repo

Type the following in your terminal:

```bash
npx create-next-app --example https://github.com/beeper/widget-example --use-yarn --app
```

This will prompt you "Ok to proceed? (y)". Type in y.

It will then ask for your project name. This will be the name of the project's folder on your computer.

Wait for packages to install. Then, type in your terminal, `cd` (the name of your project), and after that, `yarn dev`.

#### Install the widget

In Beeper, press the "Start New Chat" button:

<img alt="start-chat.png" src="./media/start-chat.png" width="500"/>

Then press "Create group chat", then "Beeper (Matrix)".

You'll see a pop-up that looks like this:

<img alt="create-room.png" src="./media/create-room.png" width="500"/>

In "Name", type something like "test room". This will be a room where you can test out your widgets during development.

Next, click the "i" icon on the top-right to open the sidebar. In the sidebar, click the blue "Add widget" text. You'll see a pop-up with a field to enter a URL. Enter `http://localhost:3000`, then press enter. Click the `x` button on the top-right of the popup.

You'll see a new widget in your sidebar. Click on it (anywhere on the background rectangle, not the buttons on the right) to open it in your sidebar.

<img alt="widget-sidebar.png" src="./media/widget-sidebar.png" width="500"/>

Wait a couple of seconds for NextJS to compile the page and for it to load. You'll then see the example widget! Click through it to explore some of the functionality, and open it up in your code editor to make changes.

To make a fully custom widget, just modify the file `app/page.tsx`. You can refer to the files in `app/examples` to see how to interact with the chat room.

### Deploying

Once you've made a widget, you can easily get it up and running on [Vercel](https://vercel.com/), the creator of NextJS, so you'll have a URL you can share with others so that they can install it to their Beeper accounts. 

Just push your code to a GitHub repo, then create an account on [vercel.com](https://vercel.com/). You'll see a screen that lets you import a Git repository. Select the one you just created, and Vercel will build and deploy your widget! It'll generate a URL that others can access it on (and you can change it to a different URL or put it on a custom domain), and every time you push to GitHub, the newest version of your widget will automatically be shown to all users.

