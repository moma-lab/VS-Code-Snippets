# How To Create Custom Code Snippets in VS Code

![Maintenance status](https://img.shields.io/badge/Maintained%3F-yes-brightgreen)
[![MIT license](https://img.shields.io/badge/license-MIT-brightgreen)](https://opensource.org/licenses/MIT)


>  With "*snippets*" [Visual Studio Code](https://code.visualstudio.com/) provides the possibility of pre-defining custom code blocks that are accessible via custom defined trigger commands. Once you type in a trigger command the built in [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) immediately provides a preview of the corresponding code snippet to insert. This is a simple and efficient way to speed up the overall code writing process. 
>
> This tutorial aims to give a brief overview on how to create, write and include such custom code snippets into VSCode.

## General Information

### Structure/Design

VS Code snippets have to be defined as *[JSON objects](https://www.w3schools.com/Js/js_json_objects.asp)*. In general they

* support C-style comments,
* can define an *unlimited number* of snippets,
* support most [TextMate](https://en.wikipedia.org/wiki/TextMate) syntax for dynamic behavior,
* intelligently format whitespace based on the insertion context,
* support different scoping methods,
* perform 'Substring matching' on trigger words (e.g. typing "fc" could match "for-const") and
* allow for easy multiline editing (via [tabstops](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_tabstops)).

### Storage location (Mac)

Custom created snippets are stored in:

> * `~/Library/Application\ Support/Code/User/snippets/` or
> * `/Users/{username}/Library/Application\ Support/Code/User/snippets/`.

### Scope & file extensions

The availability of a snippet is scoped to *one, several, or all ("global")* programming languages based on whether it is defined in:

> * a **language** specific snippet file (has a `.json` suffix, e.g. `<language>.json`) or
> * a **global** snippets file (has a `.code-snippets` suffix, e.g. `<description>.code-snippets`)
>   * *Exception*: if optional "scope" key is set to some specific languages inside a global snippet.

*Global* snippets (JSON with file suffix .code-snippets) can also be scoped to *specific projects*. Project-folder snippets are created with the '**New Snippets file for ...**' option in the **Preferences**: **Configure User Snippets** dropdown menu and are located at the root of the project in a `.vscode` folder. Project snippet files are useful for *sharing snippets* with all users working in a project. Project-folder snippets are *similar to global snippets* and can be *scoped* to specific languages through the `scope` property.

### Create a custom snippet

(1.) ***Automatically***

  * open `Code > Preferences > User Snippets` (Mac)

  * select the programming language (by [language identifier](https://code.visualstudio.com/docs/languages/identifiers)) for which the snippet should appear, or the "**New Global Snippets file ...**" option if they should appear for *all* languages

  * Write your snippet code inside the provided template, save it and you are ready to go.

(2.) ***Manually***

  * open a text editor (or use VS Code)
  * create a JSON object and define your custom snippet (see Code Snippet Examples)
  * save the file in `~/Library/Application\ Support/Code/User/snippets/` as:<br>

    * `<language>.json`, where *\<language\>* denotes the specific programming language the snippet shall be available for
    * `<description>.code-snippets`, which will make the snippet available in *all* programming languages (except the *scope* key is set to just some specific languages in the snippet object)

### Snippets versus Emmet (WIP)

> To modify/change built in code macros you can either edit the **[emmet](https://code.visualstudio.com/docs/editor/emmet) code file** (where all the macros are defined).
> 
> BUT: you need to modify this file ***EVERY single time you update VSCode*** (just a copy would suffice because emmet is not updated frequently).
> 
> It is much easier to instead write a snippet for the alternative syntax you want. Furthermore 'snippets' are part of the **VSC sync**!

- https://emmet.io/
- https://stackoverflow.com/questions/61181532/how-to-modify-emmets-tab-expansion-content
- https://stackoverflow.com/questions/61016594/linktab-shortcut-emmet-on-vscode-how-can-i-get-the-type-to-be-included-in-t/61020188#61020188
- Snippets: https://code.visualstudio.com/docs/editor/userdefinedsnippets
- Emmet (customize): https://code.visualstudio.com/docs/editor/emmet#_using-custom-emmet-snippets

## Snippet Syntax

### Basic Snippet Template

```JSON
{
  "Snippet name": {
    "scope": "Comma, separated, list, of, all, applicable, programming, languages",
    "prefix": ["All", "commands", "that", "will", "trigger", "the", "code", "snippet"],
    "body": [
      "// The actual code snippet that shall be included.",
      "const firstSnippet = {",
      "  messsage: \"Hello, world!\",",
      "  origin: \"Custom VS Code Snippet\"",
      "};"
    ],
    "description": "Insert short code snippet description here."
  }
}
```

__Explanation:__

> The key-value pairs of the JSON object above define a new code snippet.
 
* `object key` ("Snippet name"):
  * *a string displayed via IntelliSense when typing in the snippet triggering command and no `description` is provided*
 * `scope` (optional):
   * *a string of comma separated names of programming languages that are allowed to use the snippet (e.g. "scope": "javascript, typescript",)*
   * *if not specified **general scope** is expected (meaning: snippet will be available in all programming languages)*
 * `prefix` *(single string or array of string elements)*:
   * *defines one or more trigger words/commands that display the snippet in IntelliSense when typed in (Substring matching is performed on 'prefixes', so in this case, "fc" could match "for-const").*
 * `body`:
   * *array of string elements building the actual output (the code to be inserted)*
 * `description`:
   * *info string that shows up under the command in IntelliSense*

### Special `body` Constructs

> The `body` key of a snippet JSON object can use *special constructs* to *control cursors and the text being inserted*. The following constructs are supported:

* [Tabstops](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_tabstops) (e.g. `$1`, `$2` and `$0`)
* [Placeholders](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_placeholders) (e.g. `${1:foo}` or `${1:another ${2:placeholder}}`)
* [Choice](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_choice) (e.g. `${1|one,two,three|}`)
* [Variables](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_variables) (e.g. `$name` or `${name:default}`)
* [Variable transforms](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_variable-transforms)
* [Placeholder-Transform](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_placeholdertransform)
* [Transform Examples](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_transform-examples)

### Assign Keybindings to Snippets

> You can create custom [keybindings](https://code.visualstudio.com/docs/getstarted/keybindings) to insert specific snippets. Open `keybindings.json` (**Preferences: Open Keyboard Shortcuts File**), which defines all your keybindings, and add a keybinding passing `"snippet"` as an extra argument (see example here: [Assign Keybindings to Snippets](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_assign-keybindings-to-snippets)).

## Code Snippet Examples

### Real World Example #1: console.log

> Custom code snippet to log to the console in JavaScript/TypeScript (console.log), saved as global snippet file `console log.code-snippets`. `$1` and `$2` are jump marks for tab stops.

```json
{
  "Console log": {
    "scope": "javascript, typescript",
    "prefix": ["cl", "log"],
    "body": [
      "console.log('$1');",
      "$2"
    ],
    "description": "Log to the console"
  }
}
```

__Usage:__
* in your js/ts file just start typing one of the defined commands `cl` or `log`


### Real World Example #2: 'made by' console info

> Custom code snippet to create my created by credentials in JavaScript/TypeScript, saved as global snippet file `credentials.code-snippets`.

```json
{
  "moma.dev.lab credentials": {
    "scope": "javascript, typescript",
    "prefix": ["credentials", "moma", "who", "created", "made", "by"],
    "body": [
      "// Print 'created by' info to console.",
      "console.info(",
      "  '\\n                    MADE WITH ♥ BY MOMA.DEV.LAB\\n                    https://github.com/moma-lab\\n\\n'",
      ");"
    ],
    "description": "Creates 'made by...' text for console output."
  }
}
```

__Usage:__
* in your js/ts file just start typing one of the defined commands `credentials`, `moma`, `who`, `created`, `made` or `by`

### Real World Example #3: FOR-Loop

> Below is an example of a for loop snippet (including tabstops and placeholders) for JavaScript:

```JSON
// in file 'Code/User/snippets/javascript.json'
{
  "For Loop": {
    "prefix": ["for", "for-const"],
    "body": ["for (const ${2:element} of ${1:array}) {", "\t$0", "}"],
    "description": "A custom for loop."
  }
}
```

Additionally, the `body` of our example #3 has *three placeholders* (listed in order of traversal): `${1:array}`, `${2:element}`, and `$0`. You can quickly jump to the next placeholder with Tab, at which point you may edit the placeholder or jump again the next one. The string after the colon (if any) is the default text, for example element in `${2:element}`. Placeholder traversal order is ascending by number, starting from one; zero is an optional special case that always comes last, and exits snippet mode with the cursor at the specified position.

## References

* The official documentation on snippets can be found here:<br />
  https://code.visualstudio.com/docs/editor/userdefinedsnippets

* A good example overview is this 2016 article from Brandon Tillman:<br />
  https://www.brandontillman.com/vscode-create-custom-snippets/

## Contributions welcome

Created with ♥ by moma.dev.lab. Feel free to submit a pull request. Help is always appreciated.

1. [Fork this repo](https://docs.github.com/en/github/getting-started-with-github/fork-a-repo "Link to GitHub documentation on how to fork a repository")
   
2. Clone fork into your local repo:<br />
   `$ git clone https://github.com/YOUR-USERNAME/VS-Code-Snippets`

3. Create your own feature branch:<br />
   `$ git checkout -b my-branch-name`

4. Commit your changes:<br />
   `$ git commit -am 'some new feature'`

5. Push to the branch:<br />
   `$ git push origin my-new-feature`

6. [Create new Pull Request](https://docs.github.com/en/free-pro-team@latest/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request-from-a-fork)

## License

Published under the [MIT Licence](LICENSE).