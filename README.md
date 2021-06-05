# How To Create Custom Code Snippets in VS Code

![Maintenance status](https://img.shields.io/badge/Maintained%3F-yes-brightgreen)
[![MIT license](https://img.shields.io/badge/license-MIT-brightgreen)](https://opensource.org/licenses/MIT)


>  Defining code snippets is a simple way to speed up writing recurring code passages. VS Code therefor provides the possibility to create custom "shortcut commands". Once typed in they trigger the display of a predefined code snippet via IntelliSense. As a modern code editor a lot of the VS Code options are configured in JSON files. Custom code snippets are handled the same way: you create a JSON file (will be generated for you) and add your template - and that's basically all it takes.

## General

* VS Code snippets are defined as ***JSON objects***

  * support C-style comments,
  * can define an *unlimited number* of snippets,
  * support most TextMate syntax for dynamic behavior,
  * intelligently format whitespace based on the insertion context and
  * allow easy multiline editing

* Snippets scope & file extensions:

  * every snippet is scoped to one, several, or all ("global") programming languages based on whether it is defined in

    1. a **language** snippet file `.json` (e.g. \<language\>.json) or
    2. a **global** snippets file (JSON with file suffix `.code-snippets`, e.g. \<filename\>.code-snippets)

* snippet storage location (Mac):<br />
  `~/Library/Application\ Support/Code/User/snippets/` or<br />
  `/Users/{username}/Library/Application\ Support/Code/User/snippets/`

* To create a custom snippet simply do the following steps:

  * open `Code > Preferences > User Snippets` (Mac) or<br />
    `File > Preferences > User Snippets` (Win/Linux)

  * select the programming language (by [language identifier](https://code.visualstudio.com/docs/languages/identifiers)) for which the snippets should appear, or the **New Global Snippets file ...** option if they should appear for *all* languages.

  * ...

## Code Snippet Examples

### Basic Template

```json
{
  "Snippet name": {
    "scope": "Comma, separated, list, of, all, applicable, programming, languages",
    "prefix": ["All", "commands", "that", "will", "trigger", "the", "code", "snippet"],
    "body": [
      "// The actual code snippet that shall be included.",
      "const firstSnippet = {",
      "  messsage: \'Hello, world!\',",
      "  origin: \'Custom VS Code Snippet\'",
      "};"
    ],
    "description": "Insert short code snippet description here."
  }
}
```

__Explanation:__
> The above JSON object defines a new code snippet.
> 
> * "Snippet name" (=`object key`):<br />
>   *a string displayed via IntelliSense when typing in the snippet triggering command and no `description` is provided*
> * `scope` (optional):<br />
>   * *a string of comma separated names of programming languages that are allowed to use the snippet (e.g. "scope": "javascript, typescript",)*
>   * *if not specified **general scope** is expected (meaning: snippet will be available in all programming languages)*
> * `prefix` *(single string or array of string elements)*:<br />
>   * defines one or more trigger words/commands that display the snippet in IntelliSense. Substring matching is performed on 'prefixes', so in this case, "fc" could match "for-const".*
> * `body`:<br />
>   *array of string elements building the actual output (the code to be inserted)*
> * `description`:<br />
>   *info string that shows up under the command in intellisense*

### Real World Example #1: console.log

> Custom code snippet to log to the console in JavaScript/TypeScript (console.log), saved as global snippet file `console log.code-snippets`. `$1` and `$2` are jump marks for tab stops.

```json
{
  "Console log": {
    "scope": "javascript, typescript",
    "prefix": ["cl", "log"],
    "body": ["console.log('$1');", "$2"],
    "description": "Log to the console"
  }
}
```

__Usage:__
* in your js/ts file just start typing one of the defined commands `cl` or `log`


### Real World Example #2: made by console info

> Custom code snippet to create my created by credentials in JavaScript/TypeScript, saved as global snippet file `credentials.code-snippets`.

```json
{
  "moma.dev.lab credentials": {
    "scope": "javascript, typescript",
    "prefix": ["credentials", "moma", "who", "created", "made", "by"],
    "body": [
      "// Print created by info to console.",
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

### Variables and Placeholders

...

## References

* The official documentation on snippets can be found here:<br />
  https://code.visualstudio.com/docs/editor/userdefinedsnippets

* A good example overview is this 2016 article from Brandon Tillman:<br />
  https://www.brandontillman.com/vscode-create-custom-snippets/

## Contributions welcome

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

Published under the [MIT Licence](LICENSE.md).