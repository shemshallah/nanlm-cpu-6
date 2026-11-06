# nanlm-cpu-6

Disposable **NaNLMind head runner** slot 6. Holds no model weights and no
persistent data.

Dispatched by `NaNLMind/.github/workflows/ring.yml`. The job:

1. checks out `NaNLMind` and `deeprepo`;
2. builds the NaNLMind core;
3. runs `runner/nanlm_worker.py --op headshift --slot 6 --epoch <epoch>`;
4. writes one packet set for heads `h % 6 == 5` to `nanlm-ram`;
5. releases the lease; the next slot is dispatched only after this run finishes.

## Required setup (once)

Add secret `DEEPREPO_PAT` to this repo. The same token must have:

- Contents R on `NaNLMind` and `deeprepo`
- Contents RW on `nanlm-ram` and `nanlm-vram`
- Actions RW on all `nanlm-cpu-*` repos

Optional variables: `NANLM_MAIN` (default `OWNER/NaNLMind`),
`NANLM_RAM` (default `OWNER/nanlm-ram`), `NANLM_VRAM`
(default `OWNER/nanlm-vram`).

Set repository Actions variable `BSR5_PORTAL_API` to the NANNET portal base URL (HTTPS origin, without an endpoint path). The workflow exposes it as both `BSR5_PORTAL_API` and `NANNET_PORTAL_API`, with `NANNET_BOOTSTRAP_SLOT=5`. This is endpoint configuration, not an admin credential; runner authorization must use a separate per-runner credential issued by the portal.

Set repository variable `NANLM_ROUTER_URL` to the NaNROUTER Koyeb HTTPS origin and secret `NANLM_ROUTER_WORKER_TOKEN` to the worker token configured as `RIDDLE_WORKER_TOKEN` on the router. The worker acknowledges its fixed MC0:0 slot against the signed volume-registry generation using the anchored clock; it never mounts or changes a volume. The Qwen demo stays on `deeprepo-cpu-1..5`; this ring is separate.
