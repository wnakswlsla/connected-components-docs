### Table of Contents
- [Getting started on iOS/macOS with Swift](#getting-started-on-iosmacos-with-swift)
  * [1. Prepare configuration file](#1-prepare-configuration-file)
    + [Create configuration file](#create-configuration-file)
      - [Manual configuration](#manual-configuration)
      - [Using VS Code Extension](#using-vs-code-extension)
    + [Add projects or styleguides](#add-projects-or-styleguides)
      - [Manual configuration](#manual-configuration-1)
      - [Using VS Code Extension](#using-vs-code-extension-1)
    + [Add component from codebase](#add-component-from-codebase)
      - [Manual configuration](#manual-configuration-2)
      - [Using VS Code Extension](#using-vs-code-extension-2)
    + [Connect to component from Zeplin](#connect-to-component-from-zeplin)
      - [Manual configuration](#manual-configuration-3)
      - [Using VS Code Extension](#using-vs-code-extension-3)
  * [2. Install Zeplin CLI](#2-install-zeplin-cli)
  * [3. Install CLI Swift plugin](#3-install-cli-swift-plugin)
  * [4. Run Zeplin CLI](#4-run-zeplin-cli)
  * [5. Add links _(Optional)_](#5-add-links-optional)
  * [6. Connect more components](#6-connect-more-components)
- [Troubleshooting](#troubleshooting)
- [Related resources](#related-resources)

# Getting started on iOS/macOS with Swift

This guide covers how to get started with Connected Components for Swift components, on iOS or macOS.

## 1. Prepare configuration file

The first thing we'll do is to create a JSON configuration file in our repository that maps components in our codebase to the components in Zeplin.

You can either prepare the configuration file:

- Manually
- Use [Zeplin Visual Studio Code extension](https://zpl.io/vscode-extension) _(Recommended)_

In this guide, we'll prepare the file manually, while also mentioning how you can use the extension to simplify all of the steps.

### Create configuration file

#### Manual configuration

We recommend creating your Zeplin configuration file under the `.zeplin` folder in your repository, so let's create that folder first. Within the folder, also create a file called `components.json` and paste the JSON below:

```json
{
    "projects": [],
    "styleguides": [],
    "components": []
}
```

#### Using VS Code Extension

If you use the Visual Studio Code extension, it should prompt you to create the configuration file. You can also use the “Create Zeplin Configuration File” command by pressing “Command/Ctrl + Shift + P”. After creating the configuration file, make sure to click the “Login” link on top of the file to authenticate with your Zeplin account.

In a bit, we'll start filling out the configuration file:

- `projects` and `styleguides` keys are the identifiers of projects and styleguides we'll use components from.
- `components` are the Swift component files in our codebase.

### Add projects or styleguides

#### Manual configuration

Now, let's add Zeplin projects or styleguides to the configuration file. If you're using [Global Styleguides](https://blog.zeplin.io/announcing-global-styleguides-connecting-design-systems-to-engineering-65ad22bd0076), adding your styleguide(s) will be enough. If instead your components are under projects, you can add all your projects as well. In this example, we'll only add one styleguide.

To add projects or styleguides, we need their identifiers. If you're not using the Visual Studio Code extension, the easiest way to find the identifier of a Zeplin project or a styleguide is to open them in Zeplin's [Web app](https://app.zeplin.io). Look for the URL in the address bar, which should look like so: `https://app.zeplin.io/styleguide/5cd486b18a64c1414be004fb`. The identifier after `styleguide/` (or `project/`) is the identifier we're looking for.

#### Using VS Code Extension

If you're using the Visual Studio Code extension, simply click “Add styleguide” or “Add project” and you'll be presented with a list.

After adding projects or styleguides to our configuration file, it should look like so:

```json
{
    "projects": [],
    "styleguides": [
        "5cd486b18a64c1414be004fb"
    ],
    "components": []
}
```

☝️ _If you have a styleguide tree and want to connect to all the components in the tree, adding a child styleguide to the configuration should be enough._

### Add component from codebase

#### Manual configuration

Adding a component from your codebase to the configuration file is pretty straightforward—we'll add an object to the `components` list.

In this example, we'll go with a `Button.swift` file under `Example/Views`. Pick a reusable Swift component file from your codebase and let's update our configuration file to look like so:

```json
{
    "projects": [],
    "styleguides": [
        "5cd486b18a64c1414be004fb"
    ],
    "components": [
        {
            "path": "Example/Views/Button.swift",
            "zeplinNames": []
        }
    ]
}
```

#### Using VS Code Extension

If you're using the Visual Studio Code extension, click the “Add component” link which will list all the files in your repository. Pick the one you want and your configuration file should look like the below example.

```json
{
    "projects": [],
    "styleguides": [
        "5cd486b18a64c1414be004fb"
    ],
    "components": [
        {
            "path": "src/components/Button.jsx",
            "zeplinIds": []
        }
    ]
}
```

Next up, we'll populate the `zeplinIds` or `zeplinNames` key.

### Connect to component from Zeplin

#### Manual configuration

Now it's time to connect the Swift component we just added, to a component in Zeplin. We'll do that by adding the name of the component to the `zeplinNames` list.

> **`zeplinNames` is deprecated. It is recommended to use VS Code Extension to add `zeplinIds` into the configuration file. Alternatively, you can configure `zeplinNames` manually and then use VS Code Extension's `Migrate` command to convert all zeplinNames to zeplinIds later.**

Let's open the styleguide (or the project) we added and copy the name of the component in Zeplin. In our example, our component's name is “Controls / Button / Primary”. Here's how it looks like in Zeplin:

<img src="../../img/zeplinComponents.png" alt="Components in Zeplin" width="800" />

Let's add this name to the `zeplinNames` list:

```json
{
    "projects": [],
    "styleguides": [
        "5cd486b18a64c1414be004fb"
    ],
    "components": [
        {
            "path": "Example/Views/Button.swift",
            "zeplinNames": [
                "Controls / Button / Primary"
            ]
        }
    ]
}
```

Notice that in the screenshot above, we have two more states of the same button. It's possible connect a component in our codebase to multiple components in Zeplin—let's do that:

```json
{
…
            "zeplinNames": [
                "Controls / Button / Primary",
                "Controls / Button / Primary, Hover",
                "Controls / Button / Primary, Pressed"
            ]
…
}
```

Also, you can define similarly named components in a more easy way by using a wildcard in `zeplinNames` array:

```json
{
…
            "zeplinNames": [
                "Controls / Button / Primary*",
            ]
…
}
```

#### Using VS Code Extension

If you're using the Visual Studio Code extension, you can simply click “Connect to Zeplin component” and search for a component in Zeplin, directly within Visual Studio Code.

**Congratulations, we just connected our first component!** 🎉

Next up, we'll install and use Zeplin's CLI tool so that these connected components are visible in Zeplin to our team.

## 2. Install Zeplin CLI

Zeplin CLI runs in your terminal and communicates the configuration file with Zeplin.

Let's start by installing it. Zeplin CLI runs on Node.js, if you don't have it installed already, see [Node.js website](https://nodejs.org/en/).

To install Zeplin CLI from npm, run the following command on your Terminal/Command Prompt:
```sh
npm install -g @zeplin/cli
```

## 3. Install CLI Swift plugin

Zeplin CLI uses plugins to generate documentation, snippets and links from components—check out our [list of plugins](/README.md#Plugins).

Since we're using Swift, we'll install the official Swift plugin from npm, by running the following command:

```sh
npm install -g @zeplin/cli-connect-swift-plugin
```

Swift plugin also depends on SourceKitten to analyze Swift files. You can install it via Homebrew:

```sh
brew install sourcekitten
```

Now, we'll update our configuration file to use the plugin. We can do that by adding it under the `plugins` list, like so:

```json
{
    "plugins": [
        {
            "name": "@zeplin/cli-connect-swift-plugin"
        }
    ],
    "projects": [],
    "styleguides": [
        "5cd486b18a64c1414be004fb"
    ],
    "components": [
        {
            "path": "Example/Views/Button.swift",
            "zeplinNames": [
                "Controls / Button / Primary*"
            ]
        }
    ]
}
```

Next up, we'll run the CLI command!

## 4. Run Zeplin CLI

It's time! Let's **run the CLI command and see Connected Components in action** within Zeplin. 🎉

Run the following command—if it's your first time, you'll need to login to Zeplin first:

```sh
zeplin connect
```

Now head back to Zeplin and click on one of the components you connected. You should see an output similar to this:

<img src="../../img/zeplinConnectedComponent-swift.png" alt="Connected component in Zeplin" width="600" />

Our Swift plugin currently lists initializer methods and class documentations (with Markdown). If you have a different use case, please feel free to reach out to us at [support@zeplin.io](mailto:support@zeplin.io) or [contribute to the plugin](https://github.com/zeplin/cli-connect-swift-plugin).

☝️ _If you want to see the output locally in Zeplin before you publish it to your team, check out our guide on [testing your changes locally](TEST_LOCALLY.md)._

## 5. Add links _(Optional)_

Connected Components also lets you add links to various sources like your repository, wiki and so on. In the screenshot above, notice that we have a link to GitHub.

To add links to your components, check out these guides:

- [Adding repository links](../link/REPOSITORY.md), e.g. GitHub, GitLab, Bitbucket
- [Adding custom links](../link/CUSTOM.md), e.g. internal Design System wiki

## 6. Connect more components

Now that we connected our very first component, you can go ahead and connect more! Check out the section [Add component from codebase](#add-component-from-codebase) if you need any help.

Hope this getting started guide was helpful, reach out to us at [support@zeplin.io](mailto:support@zeplin.io) if you have any questions or feedback.

For further details on how to customize the configuration file, check out the [Configuration file documentation](../CONFIGURATION_FILE.md).

# Troubleshooting

If you run into any issues while running the `zeplin connect` command, make sure that you have the Swift plugin installed.

Check out [Troubleshooting](../TROUBLESHOOTING.md) for other common issues.

# Related resources

- [Zeplin CLI Swift Plugin](https://github.com/zeplin/cli-connect-storybook-plugin/blob/master/README.md)
- [Configuration file documentation](../CONFIGURATION_FILE.md)
- [Plugins](/README.md#Plugins)
- [Build your own plugin](https://github.com/zeplin/cli/blob/master/PLUGIN.md)
