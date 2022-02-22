# CmTomlMode

A better TOML mode for Codemirror. The current mode offered by codemirror v5 lacks
support for some features of TOML version 0.4 such as datetimes and inline
tables but also fails to correctly colour some of the more basic types.

You can obtain CmTomlMode via npm:

```bash
npm install @sgarciac/cm_toml_mode
```

of simply copying 
[dist/cm-toml-mode.js](https://raw.githubusercontent.com/sgarciac/cm-toml-mode/master/dist/cm-toml-mode.web.js)

 ... and writing something like this in your HTML:

```html
<link rel=stylesheet href="codemirror.css">
<script src="codemirror.js"></script>
<script src="cm-toml-mode.web.js"></script>
<script>

  CodeMirror.defineMode("bettertomlmode", CmTomlMode.tomlMode);
</script>

<style>
  .CodeMirror { height: auto; border: 1px solid #ddd; }
  .CodeMirror-scroll { max-height: 200px; }
  .CodeMirror pre { padding-left: 7px; line-height: 1.25; }
</style>

<form style="position: relative; margin-top: .5em;">
  <textarea id="demotext">
</textarea>
</form>
<script>
    var editor = CodeMirror.fromTextArea(document.getElementById("demotext"), {
      lineNumbers: true,
      mode: "bettertomlmode",
      matchBrackets: true
    });
</script>

```

You can see this mode in action [here](https://sgarciac.github.io/cm-toml-mode/)

### Usage with Codemirror 6

This mode can also be used with [Codemirror 6](https://codemirror.net/6/) ("The next 
generation of the CodeMirror in-browser editor"). It's just a matter of using the
`@codemirror/stream-parser` package, as is detailed here: https://github.com/codemirror/legacy-modes

Here is a complete example:

```js
import {StreamLanguage} from "@codemirror/stream-parser"
import {tomlMode} from "@sgarciac/cm_toml_mode";

import {EditorView, EditorState, basicSetup} from "@codemirror/basic-setup"

let view = new EditorView({
  state: EditorState.create({
    extensions: [basicSetup, StreamLanguage.define(tomlMode())]
  })
})
```

Note that, unlike the legacy modes available in the `@codemirror/legacy-modes` package,
here it's necessary to call the `tomlMode` function to obtain an object with the 
shape expected by `StreamLanguage.define` (an object with the `startState` and `token` methods).
