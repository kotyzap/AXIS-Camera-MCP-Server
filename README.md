# Axis MCP Server

### The first on-camera Model Context Protocol server — turning every Axis camera into an AI-native device.

<img width="2752" height="1536" alt="AI_Server_Bridge_for_Cameras" src="https://github.com/user-attachments/assets/e12b8a19-4ddc-4ebd-b260-6426476258d2" />

> **Any AI. Any camera. Zero cloud.**
> Install one `.eap`, and Claude, Gemini, ChatGPT, Perplexity, or your own agent can see, understand, and operate the camera — directly, at the edge.

---

<img width="1263" height="919" alt="axis-mcp-server-console-claude-connected" src="https://github.com/user-attachments/assets/64968f31-68d5-4023-8d2c-d3b19f6c5e28" />


## The breakthrough

For twenty years, integrating an IP camera meant learning VAPIX, wiring up SOAP events, wrangling digest auth, and building a bespoke backend for every project. AI agents made it worse: each one needed a custom bridge, a cloud relay, and a security review.

**Axis MCP Server collapses all of that into a single install.** It runs a standards-compliant [Model Context Protocol](https://modelcontextprotocol.io) server *inside the camera* as a native ACAP — no gateway, no middleware, no cloud round-trip. Point any MCP-capable AI client at `http://<camera-ip>/mcp` and it instantly gains a rich, structured toolset for the device.

This is edge AI in the truest sense: the intelligence talks to the camera on the camera's own loopback interface. Nothing leaves the LAN unless you choose to expose it.

---
<img width="1635" height="959" alt="claude-code-camera-view-areas" src="https://github.com/user-attachments/assets/53666828-ef9c-4005-bf0f-6cac687ad85b" />


## Why it's a category-defining, award-worthy solution

**It's genuinely first.** MCP servers run on laptops and in the cloud. This one runs *on the camera* — a Node.js runtime bundled into a native AXIS OS 12 ACAP, speaking Streamable HTTP the way modern agents expect. That combination did not exist before.

**It unlocks a massive installed base.** Axis has shipped millions of cameras; the ARTPEC-7/8/9 generation running AXIS OS 11–12 numbers in the tens of thousands per large deployment alone. A single `.eap` makes every one of them addressable by AI — no hardware change, no forklift upgrade.

**It's model-agnostic by design.** Because it speaks open MCP over HTTP, it works with Claude, Google Antigravity / Gemini, ChatGPT Developer Mode, Perplexity, Claude Code, and any custom agent — today, and whatever ships next.

**It respects the enterprise.** Reverse-proxied behind the camera's own admin authentication, with an optional bearer-guarded direct port for LAN automation. Parameter writes are allow-listed. Nothing is exposed to the internet unless the operator deliberately tunnels it.

---

## What the AI can actually do

Out of the box, the server exposes a curated toolset over MCP:

- **Device & health** — model, serial, firmware, uptime, temperature, network, and time.
- **Imaging & optics** — capture live JPEG snapshots the model can *see*, read and adjust image settings, drive zoom/focus, and run **autofocus** through a resilient four-method cascade that auto-discovers the optics and remembers what works.
- **Events & analytics** — enumerate the camera's event topics and report the state of on-device analytics like AXIS Object Analytics and motion detection.
- **Application control** — list, start, and stop ACAPs; read and (safely) update parameters.
- **The CamStreamer suite** — list and control CamStreamer streams, switch and queue CamSwitcher views, and update CamOverlay custom graphics and info-tickers. Ask an agent *"switch to the gate view and start the YouTube stream,"* and it happens.

Every tool returns clean, structured data an LLM can reason over — not raw CGI dumps.

---

<img width="576" height="893" alt="claude-code-list-installed-acaps" src="https://github.com/user-attachments/assets/7bece3fa-56b9-4921-a259-3c76df069909" />


## An operator console that's actually a joy to use

The built-in settings page is a live control room, not a form:

- **One-glance status** — app health, VAPIX connectivity, and device identity, with a one-click self-test.
- **A real-time Live Log** — every request and every MCP tool call streams in as it happens, mirrored to the AXIS system log.
- **A signature touch: the animated AI presence.** The console recognizes *which* model is connected and renders its logo in living ASCII — the Claude spark, the Gemini star, the OpenAI knot, the Antigravity mark — with an animated link pulsing between the AI and the camera and an "LLM Connected" indicator. It turns an invisible protocol into something you can watch, demo, and love.

It's the kind of detail that wins awards and wins rooms.

---
<img width="1215" height="422" alt="axis-mcp-server-installed-on-camera" src="https://github.com/user-attachments/assets/eaa61ca7-ed27-4d0a-83dc-ed0a11386176" />


## Built right

- **Native ACAP for AXIS OS 11/12** (ARTPEC-8, `aarch64`), packaged with the official ACAP Native SDK.
- **Streamable HTTP MCP**, stateless — survives the camera's respawn lifecycle cleanly.
- **From-scratch VAPIX digest client** that honors modern MD5 *and* SHA-256 challenges.
- **Bundled Node.js 20** — no runtime assumptions about the camera.
- **Robust by construction** — writable-path auto-discovery, prefix-agnostic routing, graceful degradation when a feature isn't present on a given model.

---

## Who it's for

- **Systems integrators** shipping AI-assisted surveillance without building a backend.
- **Broadcast & live-production teams** letting an agent orchestrate CamStreamer and CamSwitcher.
- **Security operations** giving analysts a natural-language line straight to the device.
- **Developers & researchers** prototyping agentic camera control in minutes, not weeks.

---

## The one-line pitch

> **Axis MCP Server puts a Model Context Protocol server inside the camera — opening tens of thousands of Axis devices to any AI, with no cloud, no middleware, and no compromise.**

*Install the `.eap`. Point your agent at the camera. Watch it come alive.*

A standalone ACAP (`.eap`) that runs a **Model Context Protocol server directly on an Axis camera**
(ARTPEC-8, AXIS OS 12 — e.g. Q1656). It exposes VAPIX-backed tools over **Streamable HTTP** so Claude
Desktop / Claude Code can connect to the camera and inspect/control it.

Built with the ACAP **Native SDK** (Node.js 20 bundled into the package). No Docker runs on the camera —
Docker is only used to *build* the `.eap` on your Mac.
<div align="center">
<img width="758" height="154" alt="mcp-client-ascii-logos" src="https://github.com/user-attachments/assets/e60d0fcc-0368-4a53-85d5-297773e98aa8" />
</div>

## Layout

```
axis-mcp-acap/
├── Dockerfile              # ACAP Native SDK build (aarch64)
├── build.sh                # docker build + docker cp -> .eap
├── plan.md                 # design plan
└── app/
    ├── manifest.json        # schemaVersion 1.7.4; appName axis_mcp
    ├── axis_mcp             # launcher (filename == appName)
    ├── Makefile             # no-op (acap-build runs make)
    ├── LICENSE
    ├── package.json         # main: dist/bootstrap.js
    ├── tsconfig.json
    ├── src/
    │   ├── bootstrap.ts     # HTTP servers: /mcp, /status.cgi, /settings.cgi
    │   ├── mcpServer.ts      # McpServer + tool registration
    │   ├── vapix.ts          # Digest auth client (MD5 + SHA-256)
    │   ├── settings.ts       # persisted config (PERSISTENT_DATA_PATH)
    │   └── tools/            # device, imaging, events, apps
    └── html/index.html      # settings UI (light default + theme switcher)
```

## Tools (v1)

| Tool | VAPIX |
|---|---|
| `get_device_info` | basicdeviceinfo.cgi (getAllProperties) |
| `get_system_status` | param.cgi (Network/Brand/Firmware) + temperaturecontrol.cgi |
| `get_time` | time.cgi |
| `take_snapshot` | jpg/image.cgi → MCP image content |
| `get_image_settings` / `set_image_settings` | param.cgi ImageSource/Image |
| `get_optics` | opticscontrol.cgi getOptics (ids + capabilities) |
| `autofocus` | cascade: opticscontrol.cgi performAutofocus → startFocusSearch → opticssetup.cgi → ptz.cgi?autofocus=on (caches the working method) |
| `set_zoom_focus` | opticscontrol.cgi setMagnification / autofocus cascade |
| `list_event_declarations` | SOAP GetEventInstances |
| `get_analytics_status` | applications/list.cgi (filtered) |
| `list_apps` / `control_app` | applications/list.cgi, control.cgi |
| `get_params` / `set_param` | param.cgi (writes allowlisted) |

### CamStreamer suite (require the respective ACAP installed)

| Tool | Endpoint |
|---|---|
| `camstreamer_list_streams` | /local/camstreamer/stream/list.cgi |
| `camstreamer_stream_status` | /local/camstreamer/stream/status.cgi |
| `camstreamer_control_stream` | /local/camstreamer/stream/set.cgi (start/stop) |
| `camswitcher_list_playlists` | /local/camswitcher/playlists.cgi?action=get |
| `camswitcher_switch_playlist` | /local/camswitcher/playlist_switch.cgi |
| `camswitcher_queue_playlist` | /local/camswitcher/playlist_queue_push.cgi |
| `camswitcher_get_queue` / `camswitcher_play_next` / `camswitcher_clear_queue` | playlist_queue_*.cgi |
| `camswitcher_output_info` / `camswitcher_list_clips` | output_info.cgi, clips.cgi |
| `camoverlay_list_services` | /local/camoverlay/api/services.cgi |
| `camoverlay_set_service_enabled` | services.cgi?action=set |
| `camoverlay_update_graphic_text` | customGraphics.cgi?action=update_text |
| `camoverlay_infoticker` | infoticker.cgi |

These call the CamStreamer/CamSwitcher/CamOverlay CGIs on the same camera using the same digest auth.
If the corresponding ACAP isn't installed, the tool returns a clear "not installed" error.

## Build

Requires Docker Desktop on your Mac.

```sh
cd axis-mcp-acap
sh build.sh arm64        # -> Axis_MCP_Server_1_0_0_aarch64.eap (or axis_mcp_*.eap)
```

## Install

Camera UI → **System → Apps** → enable *Allow unsigned apps* → **Add app** → upload the aarch64 `.eap` →
**Start**. Open the app's settings page, enter VAPIX admin credentials, click **Run self-test**.

## Connect an MCP client

Two endpoints:

- Reverse-proxied (camera enforces admin digest auth):
  `http://<camera-ip>/local/axis_mcp/mcp`
- Direct LAN port (for clients without digest; optional bearer token):
  `http://<camera-ip>:8000/mcp`

```sh
claude mcp add --transport http axis-q1656 http://<camera-ip>:8000/mcp
```

Inspect with: `npx @modelcontextprotocol/inspector`

## Verification checklist

- [ ] `.eap` installs and starts on FW 12.x without manifest errors
- [ ] `curl -X POST http://<ip>:8000/mcp` with an `initialize` body returns server info
- [ ] MCP Inspector lists all tools and each returns live data
- [ ] `take_snapshot` returns a viewable JPEG
- [ ] `set_param` refuses non-allowlisted groups
- [ ] App survives respawn (save settings → app restarts → MCP still answers)
- [ ] Clean SIGINT exit

## Notes

- Digest auth honours the `algorithm` directive (MD5 **and** SHA-256 + `-sess`) — an MD5-only client
  gets a silent 401 on modern AXIS OS.
- The in-app server binds `process.env.HTTP_PORT` (AXIS OS assigns it; 32554 only for local dev).
- Direct-port reachability from the LAN depends on the camera firewall; the reverse-proxied path is the
  sanctioned route. Keep a bearer token set if the direct port is enabled.
