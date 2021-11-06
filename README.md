# Auto-Formatting in shared projects

State: Work in progress

How to start easily with Auto-Formatting in shared team projects in VSCode?

First of all, the auto format shortcut in Visual Studio Code is the following:

- Windows: `Shift + Alt + F`
- Mac: `Shift + Option + F`
- Linux: `Ctrl + Shift + I`

or `Right click -> Format`.

⚠️ BUT: The result of auto-formatting depends on your vscode settings as well as the used formatter (extension). Therefore a shared configuration for yor team is needed.

(You can find out which formatter is currently used by `Right click -> Format Document with ...`)

## How to use the same setting in teams?

The most simple way I found is using a combination of `.editorconfig` and `.vscode/settings.json`. For editorconfig support the [EditorcConfig-extension](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig) needs to be installed.

If you open up a project folder, .editorconfig and setting.json will pe parsed by Visual Studio Code.

### Separation of concerns

Why two config files? I followed this argument from [theodo.com - Set up ESlint, Prettier & EditorConfig without conflicts](https://blog.theodo.com/2019/08/empower-your-dev-environment-with-eslint-prettier-and-editorconfig-with-no-conflicts/):

- All configuration related to the editor (end of line, indent style, indent size...) should be handled by EditorConfig
- Everything related to code formatting should be handled by [a Formatter extension such as] Prettier

(_The article also refers to "The rest (code quality) should be handled by ESLint" as well, but we'll skip this for an easy start._)

### Extensions needed

- Parse .editorconfig: [EditorConfig](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)
- Formatter, e.g. [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

_Recommended extensions can be documented in [.vscode/extensions.json](./.vscode/extensions.json)_

### Configuration

1. Line endings and encoding UTF-8 for all files are defined in [.editorconfig](./.editorconfig), advantage: other IDEs and other file type formatters can parse it as well

```yaml
[*]
end_of_line = lf
charset = utf-8
```

We leave the rest of the settings up to the formatter extensions.

2. Set a defaultFormatter for your team such as [Prettier](https://prettier.io/) for HTML/CSS/JS in [.vscode/settings.json](./.vscode/settings.json)

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "[php]": {
    "editor.defaultFormatter": null
  }
}
```

Just use prettier defaults by using empty [.prettierrc](./.prettierrc) file, no discussions (at first ;-)). Prettier default is for example tab width = 2, see [Prettier options](https://prettier.io/docs/en/options.html).

```json
{}
```

Optional: Exclude file types if you need a different formatter for PHP, Nunjucks, etc. (Formatters behave differently, some respect and parse .editorconfig). Here is an example for choosing a different formatter for PHP:

```json
"[php]": {
    "editor.defaultFormatter": "bmewburn.vscode-intelephense-client"
    }
```

3. If you don't want to impose editor.formatOnSave on true for all file types, you can exclude them (or just activate it for specific filetypes):

```json
 "[scss]": {
        "editor.formatOnSave": true
    }
```

_Unfortunately you can't currently configure multiple languages at once like [scss, js], see [vscode/issues/51935](https://github.com/microsoft/vscode/issues/51935)._

If you want to let decide each team member when to use formatOnSave, you can use [Status Bar Format Toggle extension](https://marketplace.visualstudio.com/items?itemName=tombonnike.vscode-status-bar-format-toggle) as well and set `formatOnSave`to `false`.

### Challenges:

While Prettier supports many file types, there are always exceptions such as Nunjucks (used by eleventy e.g.), PHP (only via prettier plugin).

Also file auto detection can be an issue, check status bar for result of autodection:

### More resources:

- https://blog.theodo.com/2019/08/empower-your-dev-environment-with-eslint-prettier-and-editorconfig-with-no-conflicts/
- (german) https://bitspeicher.blog/make-it-prettier-teil-1-was-ist-prettier-und-warum-sollte-ich-es-verwenden/
