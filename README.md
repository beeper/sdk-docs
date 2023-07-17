# Beeper SDKs
This repo will contain instructions for various SDKs that can be used with Beeper.

# Widgets

Widgets are webpages that live on the sidebar of Beeper. They can access data from a chat in Beeper, such as messages and a list of users. They can also take actions, such as sending messages on the user's behalf. 

## Get started in 5 minutes

We'll set you up with a [NextJS](https://nextjs.org/) project that will provide a foundational codebase you can then build upon.


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

## Build a widget

### Develop

The example project you just cloned contains the foundations for widget-making. You can build your widget directly by modifying the codebase there. 

The codebase is built using [NextJS](https://nextjs.org/). If you haven't already used NextJS, don't worry - NextJS is just a layer on top of React that makes the developer experience better. It handles things like routing, hot-reloading, server-side rendering, performance optimizations, and various other things. 

If it is your first time on NextJS, you may want to check out the [NextJS Docs](https://nextjs.org/docs), specifically [Routing](https://nextjs.org/docs/app/building-your-application/routing). The codebase has inbuilt support for [Tailwind CSS](https://tailwindcss.com/) and [CSS Modules](https://nextjs.org/docs/app/building-your-application/styling/css-modules). You can also use Sass via [these instructions](https://nextjs.org/docs/app/building-your-application/styling/sass).

The page immediately rendered when you open your widget is `app/page.tsx`. You can either write your React app all within that page, or make and import components. 

Before being able to access data from the chat room, you'll need to request permissions via the widget. You can do this via the array of `capabilities` passed to `MuiCapabilitiesGuard` in `app/page.tsx`.

That's it! A widget really is just an embedded webpage that can interact with data from the chat room, so just like with regular websites, you have complete freedom on what you want to build. 

### Examples

The example repo you just cloned: https://github.com/beeper/widget-example

Summarizer: a  widget that finds the last message you read and then fetches and summarizes your unread messages. https://github.com/beeper/widget-summarizer

"Do It": a widget that's like a universal smart button. You press it and it guesses what you need based on the conversation. https://github.com/beeper/widget-do-it
### Ship

Once you've made a widget, you can easily get it up and running on [Vercel](https://vercel.com/), the creator of NextJS, so you'll have a URL you can share with others so that they can install it to their Beeper accounts. 

Just push your code to a GitHub repo, then create an account on [vercel.com](https://vercel.com/). You'll see a screen that lets you import a Git repository. Select the one you just created, and Vercel will build and deploy your widget! It'll generate a URL that others can access it on (and you can change it to a different URL or put it on a custom domain), and every time you push to GitHub, the newest version of your widget will automatically be shown to all users.