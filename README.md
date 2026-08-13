# OO-LD Playground (YAML mode with Python code generation)

An interactive playground for [OO-LD](https://oo-ld.org/) schemas that edits the schema and data as YAML and additionally generates Python code from the schema, in the browser, via [Pyodide](https://pyodide.org/). It combines a JSON Schema form editor ([json-editor](https://github.com/json-editor/json-editor)) with the [JSON-LD playground](https://github.com/json-ld/json-ld.org), so a single OO-LD document can be exercised as a form, as linked data, and as a Python model at the same time.

Live: <https://oo-ld.github.io/playground-python-yaml/>

## Why YAML and Python

YAML is a more human-friendly way to author the same document. OO-LD keeps JSON as the canonical notation, so YAML here is expected to stay within the JSON-compatible subset (no anchors, tags, or implicit-typing surprises); everything is parsed back to the identical JSON structure. See the [Notation](https://oo-ld.org/latest/spec/#notation) section of the specification for the normative rule.

The Python pane shows how an OO-LD schema maps to typed Python (for example pydantic models), which is how a schema becomes usable as function and API signatures. The code runs entirely in the browser through Pyodide.

## What the panes show

- **Schema** - the OO-LD document being edited (YAML). Press "Set Schema" to apply your edits.
- **Form** - the user interface generated from the schema by json-editor.
- **Data** - the instance produced by the form, shown as YAML.
- **Python** - Python code generated from the current schema.
- **Validation** - JSON Schema validation errors for the current instance.
- **JSON-LD** - the instance interpreted as linked data, showing the expanded form and the resulting RDF.

The first load takes a while: Pyodide downloads a Python runtime and installs packages in the browser before the Python pane works.

## The default example

The playground opens with the official OO-LD `Person` example, fetched at page load from the canonical deployment:

<https://oo-ld.org/latest/schemas/Person.schema.json>

`Person` builds on `Thing` through both `allOf` (so JSON Schema validators apply the base rules) and `@context` (so JSON-LD resolves the inherited term mappings). That pairing is the core OO-LD inheritance idiom.

Loading the example over the network rather than vendoring a copy keeps the playground in step with the specification. Because the published schemas use relative references such as `"Thing.schema.json"`, the playground rewrites them to absolute URLs against `https://oo-ld.org/latest/schemas/` before handing the document to the editor. If the deployment cannot be reached, the playground falls back to a small offline copy of the `Minimal` example and logs a warning to the browser console.

The full official example set is browsable at <https://github.com/OO-LD/oold-schema/tree/main/examples> and served under <https://oo-ld.org/latest/schemas/>. To try any of them, paste the schema into the Schema pane and press "Set Schema".

## Sharing what you built

The "Direct Link" button encodes the current schema, data and editor options into the URL as a `?data=` parameter (JSON, LZString-compressed to base64). Anyone opening that link gets your exact state. A `?data=` link always takes precedence over the default example, so shared links keep working unchanged.

"Reset Playground" clears the parameters and returns to the default example.

## Related playgrounds

| Playground | Purpose |
|---|---|
| [playground](https://oo-ld.github.io/playground/) | JSON editing mode |
| [playground-yaml](https://oo-ld.github.io/playground-yaml/) | YAML editing mode |
| [playground-python-yaml](https://oo-ld.github.io/playground-python-yaml/) | YAML mode plus in-browser Python code generation (this one) |
| [playground-awl](https://oo-ld.github.io/playground-awl/) | Abstract Workflow Language |

## Local development

This is a static page with no build step. The JSON-LD playground is included as a git submodule:

```bash
git clone --recurse-submodules https://github.com/OO-LD/playground-python-yaml.git
cd playground-python-yaml
python -m http.server
```

Then open <http://localhost:8000/>. Serving over HTTP rather than opening `index.html` directly matters, because the default example is fetched with `fetch()`.

## More about OO-LD

- Documentation: <https://oo-ld.org/>
- Specification: <https://oo-ld.org/latest/spec/>
- Main repository: <https://github.com/OO-LD/oold-schema>
- Python library: <https://github.com/OO-LD/oold-python>
