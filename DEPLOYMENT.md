# Cloudflare Workers Configuration

This file contains the configuration for deploying to Cloudflare Workers.

## Setup Instructions

1. **Create a Cloudflare Account**
   - Go to https://dash.cloudflare.com/
   - Sign up or sign in

2. **Install Wrangler CLI**
   ```bash
   npm install -g @cloudflare/wrangler
   ```

3. **Authenticate with Cloudflare**
   ```bash
   wrangler login
   ```
   This will open a browser window to authenticate your account.

4. **Update wrangler.toml**
   - Replace `name = "fuse-extractor"` with your desired worker name
   - Update routes if you have a custom domain

5. **Deploy**
   ```bash
   npm run deploy
   ```

## Environment Configuration

### Development
```bash
wrangler dev
```
Runs locally at http://localhost:8787

### Production
Set environment variables in `wrangler.toml`:
```toml
[env.production]
name = "fuse-extractor"
routes = [
  { pattern = "*.fuse-extractor.dev", zone_name = "fuse-extractor.dev" }
]
```

## Secrets Management

To add secrets (API keys, etc.):

```bash
wrangler secret put SECRET_NAME
```

Then reference in code:
```typescript
export interface Env {
  SECRET_NAME?: string;
}

export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const secret = env.SECRET_NAME;
    // ...
  }
}
```

## Monitoring

View real-time logs:
```bash
wrangler tail
```

## Rate Limiting & Limits

Default Cloudflare Workers limits (free tier):
- 100,000 requests/day
- 50ms CPU time per request
- 30MB max bundle size

Upgrade to paid plan for higher limits.

## Rollback

To rollback to a previous version:
```bash
wrangler rollback --message "Rollback message"
```

## Documentation

- [Cloudflare Workers Documentation](https://developers.cloudflare.com/workers/)
- [Wrangler CLI Reference](https://developers.cloudflare.com/workers/wrangler/commands/)
- [KV Namespace Documentation](https://developers.cloudflare.com/workers/runtime-apis/kv)
