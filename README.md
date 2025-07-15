# candy-slerf-auth
Injects Slerf loyalty headers into HTTP responses for downstream token-gated services, Web3 auth flows, and bot filtering across Boomchainlab infrastructure.


# 🔐 caddy-slerf-auth

**A lightweight Caddy plugin by Boomchainlab that injects custom loyalty headers for Web3 authentication, tracking, and downstream token-gated systems.**

---

## 📌 Overview

`caddy-slerf-auth` is an HTTP handler module for [Caddy v2](https://caddyserver.com/) that adds a custom header (`X-Slerf-Auth: Verified`) to all HTTP responses.

This is useful in:
- Token-gated frontends (e.g., CHONK9K or SLERF portals)
- Web3 apps requiring wallet-aware routing
- Loyalty systems or referrals built on $SLERF

---

## 🧩 Module ID

http.handlers.slerfauth
---

## ⚙️ Installation

Use [xcaddy](https://github.com/caddyserver/xcaddy) to build Caddy with this plugin:

```bash
xcaddy build \
  --with github.com/Boomchainlab/caddy-slerf-auth/v2


🚀 Example: Using in a Caddyfile
boomchainlab.blog {
  route {
    slerfauth
    respond "Loyalty Verified"
  }
}

This configuration:
	•	Injects X-Slerf-Auth: Verified into every response
	•	Sends a plain response for demo purposes

⸻

📂 Project Structure
	•	plugin.go — Main plugin code
	•	go.mod — Go module with Caddy dependency
	•	plugin_test.go — Unit tests (optional)
	•	.github/workflows/ci.yml — GitHub Actions CI for build/test



🛠 Development
Build locally
go build


Run tests:
go test ./...

✅ Use Cases
Scenario
How It Helps
Web3 user loyalty
Inject loyalty status into responses
Token-based feature access
Frontends check headers for verification
Slerf-powered referral engines
Track & trace request sources securely


🪪 License

MIT © Boomchainlab

---

## 📄 `plugin.go`

```go
package slerfauth

import (
	"net/http"

	"github.com/caddyserver/caddy/v2"
	"github.com/caddyserver/caddy/v2/modules/caddyhttp"
)

func init() {
	caddy.RegisterModule(SlerfAuth{})
}

type SlerfAuth struct{}

func (SlerfAuth) CaddyModule() caddy.ModuleInfo {
	return caddy.ModuleInfo{
		ID:  "http.handlers.slerfauth",
		New: func() caddy.Module { return new(SlerfAuth) },
	}
}

func (m SlerfAuth) Provision(caddy.Context) error {
	return nil
}

func (m SlerfAuth) Validate() error {
	return nil
}

func (m SlerfAuth) ServeHTTP(w http.ResponseWriter, r *http.Request, next caddyhttp.Handler) error {
	w.Header().Set("X-Slerf-Auth", "Verified")
	return next.ServeHTTP(w, r)
}


