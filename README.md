# kubectl-doc

<p align="center">
  <img src="docs/assets/kubectl-doc-demo-1.gif" alt="kubectl-doc interactive terminal demo" width="48%">
  <img src="docs/assets/kubectl-doc-demo-2.gif" alt="kubectl-doc web documentation demo" width="48%">
</p>

`kubectl-doc` renders Kubernetes OpenAPI v3 schemas as YAML-shaped
documentation for terminal, Markdown, and interactive documentation surfaces.

Unlike `kubectl explain`, `kubectl-doc` is not a single-struct field lookup.
`kubectl explain --recursive` prints a flattened tree, but it is not valid
YAML, has no useful folding, and does not give an authoring-oriented view of a
resource. `kubectl-doc` renders the schema as YAML-shaped documentation, with
optional fields commented, required fields visible, and interactive TUI plus web
views for browsing the full resource.

## Installation

Install with Homebrew:

```shell
brew install sttts/tap/kubectl-doc
```

Install with Krew:

```shell
kubectl krew install doc
```

The installed `kubectl-doc` binary is available to `kubectl` as `kubectl doc`.

## Command Reference

```shell
kubectl doc [resource] [flags]
```

Examples:

```shell
kubectl doc
kubectl doc deployments
kubectl doc deployments -o kro
kubectl doc deployments -o markdown
kubectl doc deployments -o markdown --all-versions
kubectl doc deployments -o html > deployment.html
kubectl doc -w
kubectl doc -f ./crd.yaml
kubectl doc -f ./crd.yaml --version v1
kubectl doc -f ./crd.yaml -o jsonschema
kubectl doc -f ./crd.yaml -o kro --all-versions
```

Resource selectors in cluster mode follow Kubernetes resource lookup syntax,
including plural names, singular names, kinds, short names, and qualified forms
such as `deployments.apps` or `deployments.v1.apps`.

Flags:

| Flag | Description |
| --- | --- |
| `-f, --filename <path>` | Read a local CRD manifest instead of cluster discovery/OpenAPI. |
| `-o, --output <format>` | Select output format. Default: `yaml`. |
| `--nocolor` | Disable color in YAML overview/schema output. |
| `--version <version>` | Select a served CRD version when using `-f`. |
| `--all-versions` | Render all served versions for output formats that support it. |
| `--expand-depth <n>` | Initial static expansion depth for YAML-shaped examples. Default: `2`. |
| `--descriptions=false|required|true` | Control description comments/markers. Default: `true`. |
| `--columns <n>` | Target width for terminal comment and Markdown paragraph wrapping. Markdown defaults to terminal width, otherwise `80`; YAML defaults to terminal width when available. |
| `--field-details` | Include Markdown field detail sections. Default: disabled. |
| `--disable-filtering` | Disable generated filtering UI/index data for static interactive documentation such as `markdown-fern`. |
| `--fern-schema-dir <dir>` | Write `markdown-fern` full schema JSON sidecars for lazy loading. |
| `--fern-schema-url-path <path>` | Relative URL prefix used by `markdown-fern` to load generated schema JSON sidecars. |
| `-i, --interactive` | Shortcut for `-o tui`. |
| `-w, --web` | Shortcut for `-o browser`; best-effort opens the localhost URL in a default browser when possible. |

Output formats:

| Format | Status | Description |
| --- | --- | --- |
| `yaml` | implemented | Manifest-shaped, syntactically valid YAML documentation. |
| `jsonschema` | implemented | Plain JSON Schema YAML without rendered documentation markup. |
| `kro` | implemented | Kro SimpleSchema-style YAML schema view. |
| `html` | implemented | Self-contained interactive HTML for a selected resource or CRD. |
| `markdown`, `markdown-github` | implemented | GitHub Markdown page with fenced YAML examples. |
| `markdown-fern` | implemented | Fern-compatible MDX page with an embedded schema component payload and optional lazy schema sidecars. |
| `browser` | implemented | Localhost browser server with discovery navigation and lazy schema loading. |
| `tui` | implemented | Interactive terminal view. |

## Generated Examples

- GitHub Markdown:
  [`docs/examples/github-dynamographdeployment.md`](docs/examples/github-dynamographdeployment.md)
- Interactive HTML:
  [`docs/examples/html-dynamographdeployment.html`](https://sttts.github.io/kubectl-doc/examples/html-dynamographdeployment.html)
- Kro SimpleSchema:
  [`docs/examples/kro-dynamographdeployment.yaml`](https://sttts.github.io/kubectl-doc/examples/kro-dynamographdeployment.yaml)

## GitHub Markdown Example

The compact example below is generated from
[`internal/cli/testdata/dynamographdeployment-light-crd.yaml`](internal/cli/testdata/dynamographdeployment-light-crd.yaml)
with
`kubectl-doc -o markdown-github --all-versions --descriptions=true --expand-depth=4 --columns=100`.
The linked generated examples above are generated from the full DynamoGraphDeployment CRD fixture
[`internal/cli/testdata/dynamographdeployment-crd.yaml`](internal/cli/testdata/dynamographdeployment-crd.yaml).

<!-- BEGIN GENERATED GITHUB MARKDOWN EXAMPLE -->
# DynamoGraphDeployment

| Field | Value |
| --- | --- |
| API Version | `nvidia.com/v1beta1` |
| Kind | `DynamoGraphDeployment` |
| Resource | `dynamographdeployments` |

## YAML

<details open>
<summary>YAML</summary>

```yaml
# DynamoGraphDeployment declares a model serving graph managed by the Dynamo operator.
apiVersion: nvidia.com/v1beta1
kind: DynamoGraphDeployment
metadata:
  name: "<name>"
  namespace: "<namespace>"
# Spec defines the desired graph topology and pod-level defaults for each component.
spec: # required
  # Components are the named graph nodes reconciled into Kubernetes workloads.
  components: # required, listType: map, listMapKeys: name
    - # Unique component name used for generated workload and service names.
      name: "<string>" # required, minLength: 1, maxLength: 63

      # Pod template fragment merged with operator defaults before creating pods.
      # podTemplate: {} # preserveUnknownFields

      # Desired number of pod replicas for the component.
      # replicas: 1 # default, minimum: 0

      # Container resource requests and limits for the component.
      # resources:
        # Resource limits keyed by Kubernetes resource name.
        # limits:
          # <key>: "<string>"

        # Resource requests keyed by Kubernetes resource name.
        # requests:
          # <key>: "<string>"

      # Service ports exposed by this component.
      services: # optional
        - # Service port name.
          name: "<string>" # required, minLength: 1

          # Service port number.
          port: <int32> # required, minimum: 1, maximum: 65535

          # Network protocol for the service port.
          # protocol: "TCP" # default, enum: "UDP"

      # Shared memory size mounted into the component pod.
      # sharedMemorySize: <int-or-string> # intOrString

  # Annotations propagated to generated workloads unless a component overrides them.
  # annotations:
    # <key>: "<string>"

  # Backend framework used when a component does not override the runtime explicitly.
  # backendFramework: "sglang" # default, enum: "vllm" | "trtllm"

  # Environment variables applied to every component unless a component overrides them.
  envs: # optional
    - # Name of the environment variable.
      name: "<string>" # required, minLength: 1

      # Literal value for the environment variable.
      # value: "<string>"

      # Source for the environment variable value.
      valueFrom: # optional
        # Selects a key from a Secret in the same namespace.
        secretKeyRef: {} # optional, show with --expand-depth 5

# Status summarizes the observed deployment state.
# status: {}
```
</details>
<!-- END GENERATED GITHUB MARKDOWN EXAMPLE -->

## Fern Integration

`markdown-fern` emits a Fern-compatible MDX page for a selected Kubernetes
resource or CRD. By default the generated MDX embeds the schema payload in the
page. With `--fern-schema-dir`, it embeds only the shallow initial payload and
references generated static full-payload JSON sidecars. The rendered documentation
does not fetch OpenAPI data after page load.

```shell
kubectl doc -f ./crd.yaml -o markdown-fern > fern/pages/reference/my-resource.mdx
kubectl doc -f ./crd.yaml -o markdown-fern --all-versions > fern/pages/reference/my-resource.mdx
```

The reusable React component source is shipped in this repository under
[`react/kubectl-doc`](react/kubectl-doc). Run `make gen` before copying or
vendoring that directory into the Fern project so the generated page can import
both the component and its generated bundler artifacts:

```tsx
import { KubeSchemaDoc } from "@/components/kubectl-doc/KubeSchemaDoc";
```

```shell
make gen
```

`make gen` creates the component-local `kubectl-doc-runtime.js` bundler artifact
from the single authoritative runtime source at
`internal/render/web/assets/kubectl-doc.js`. Do not edit or maintain a second
runtime copy.

Fern is one host for the generic React component. The Fern page should import
`KubeSchemaDoc` directly and let that component mount the shared runtime.
Filtering is enabled by default; disable it for smaller generated pages:

```shell
kubectl doc -f ./crd.yaml -o markdown-fern --disable-filtering > fern/pages/reference/my-resource.mdx
```

For large resources, generate full schema JSON sidecars as static Fern assets and
let the page embed only the shallow initial tree:

```shell
mkdir -p fern/assets/kubectl-doc/schemas
kubectl doc -f ./crd.yaml -o markdown-fern \
  --fern-schema-dir fern/assets/kubectl-doc/schemas \
  --fern-schema-url-path /assets/kubectl-doc/schemas \
  > fern/pages/reference/my-resource.mdx
```

`markdown-fern` is MDX, not standalone HTML. Publish it through Fern like any
other Fern documentation page. If your Fern setup supports static export, the
exported site can be hosted on static infrastructure such as GitHub Pages. For a
single self-contained file that can be published directly, use `-o html`.

## React Integration

`KubeSchemaDoc` is a thin React lifecycle adapter around the shared
`kubectl-doc` browser runtime. The standalone HTML renderer remains the
blueprint for the DOM behavior; React-based documentation projects should
consume this component rather than reimplementing schema-line rendering.

Use the component in any React application by vendoring or packaging
[`react/kubectl-doc`](react/kubectl-doc). The `schema` prop is the same
structured JSON payload that `markdown-fern` embeds in MDX or writes as static
sidecars with `--fern-schema-dir`:

```tsx
import { KubeSchemaDoc, type KubeSchemaDocument } from "./kubectl-doc/KubeSchemaDoc";

export function ResourceSchema({ schema }: { schema: KubeSchemaDocument }) {
  return <KubeSchemaDoc data={schema} filtering />;
}
```

For large schemas, pass a shallow payload with `fullPayloadURL`. The component
renders the shallow tree first, then preloads and indexes that static JSON
payload in the background. The visible tree stays shallow until the user
filters or expands unloaded fields, but the full payload is usually ready by
then:

```tsx
<KubeSchemaDoc data={schema} filtering />
```

If the host needs custom loading, provide the single v1 loading hook:

```tsx
<KubeSchemaDoc
  data={schema}
  filtering
  loadFullSchema={() => fetch(schema.fullPayloadURL!).then((response) => response.json())}
/>
```

Set `preloadFullSchema={false}` only when the host page needs to defer network
traffic until interaction. The default is to preload after the initial shallow
render.

Embedded React/Fern pages also activate the schema widget by default so the
details overlay is visible for the initially selected `apiVersion` field. Set
`autoFocus={false}` only when the host page should keep keyboard focus
elsewhere on load.

Static documentation embeddings can opt into the same behavior with
`data-kdoc-auto-focus="true"` on the mounted `data-kubectl-doc` element.

The React component owns only lifecycle, style injection, and optional static
payload loading. Folding, filtering, details, keyboard navigation, syntax
highlighting, comment wrapping, and DOM updates belong to the shared browser
runtime generated by `make gen`.
