# ModelMirror — AI Code Companion

An intelligent Chrome extension that explains, refactors, optimizes, and generates tests for code using AI. The backend is deployed on Vercel, eliminating the need to run a local server.

## Features

- **Explain Code**: Get clear, educational explanations of code snippets
- **Refactor**: Improve code structure and readability
- **Optimize**: Identify performance bottlenecks and optimization strategies
- **Find Bugs**: Detect security issues and logical errors
- **Generate Tests**: Automatically create test cases with edge case coverage
- **Selection Capture**: Quickly grab code from any webpage

## Quick Start

### For Extension Installation

1. **Load the extension in Chrome:**
   - Open `chrome://extensions`
   - Enable "Developer mode" (top right)
   - Click "Load unpacked"
   - Select this folder
   - The extension is ready to use!

2. **Configure your OpenRouter API key:**
   - Open the extension's side panel
   - Look for the server status indicator
   - Your Vercel backend is already connected

### For Server Deployment (Vercel)

1. **Fork/clone this repository to your computer**

2. **Create a `.env` file in the root directory:**
   ```bash
   cp .env.example .env
   ```
   Edit `.env` and add your OpenRouter API key:
   ```
   OPENROUTER_API_KEY=your_key_here
   OPENROUTER_MODEL=openai/gpt-4o-mini
   ```

3. **Install and test locally:**
   ```bash
   npm install
   node server.js
   ```
   The server should run on `http://localhost:3000`

4. **Deploy to Vercel:**

   **Option A: Using Vercel CLI**
   ```bash
   npm install -g vercel
   vercel
   ```
   Follow the prompts and add your `OPENROUTER_API_KEY` as an environment variable.

   **Option B: Using GitHub + Vercel Dashboard**
   - Push this repo to GitHub
   - Go to [vercel.com](https://vercel.com)
   - Click "New Project"
   - Import from GitHub
   - Add `OPENROUTER_API_KEY` in Environment Variables
   - Deploy

5. **Update the extension with your Vercel URL:**
   - After deployment, you'll get a URL like `https://your-project.vercel.app`
   - Open [background.js](./background.js) (line ~5)
   - Replace `https://modelmirror.vercel.app` with your actual Vercel URL
   - Reload the extension in Chrome

## API Endpoints

### `POST /analyze`
Analyzes code using AI based on the requested action.

**Request:**
```json
{
  "code": "const x = 5;",
  "action": "explain|refactor|optimize|generate_tests|find_bugs",
  "language": "javascript",
  "context": { "purpose": "...", "expected_output": "...", "edge_cases": "..." }
}
```

**Response:**
```json
{
  "title": "Quick explanation",
  "explanation": "Detailed analysis",
  "fix_before": "original code",
  "fix_after": "improved code",
  "why": "Key insight",
  "intent": "Action taken"
}
```

## Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `OPENROUTER_API_KEY` | ✅ Yes | Your OpenRouter API key |
| `OPENROUTER_MODEL` | ❌ No | AI model (default: `openai/gpt-4o-mini`) |
| `OPENROUTER_SITE_URL` | ❌ No | Your site URL for OpenRouter analytics |
| `OPENROUTER_APP_NAME` | ❌ No | Your app name for OpenRouter analytics |

## Project Structure

```
├── background.js          # Chrome service worker
├── content.js            # Content script
├── sidepanel.html        # Extension UI
├── sidepanel.js          # UI logic
├── server.js             # Express backend (Vercel)
├── manifest.json         # Extension manifest
├── vercel.json          # Vercel configuration
├── .env.example         # Environment template
├── .gitignore           # Git ignore rules
└── icons/               # Extension icons
```

## Development

### Local Testing

1. Install dependencies:
   ```bash
   npm install
   ```

2. Create `.env` with your API key

3. Start the local server:
   ```bash
   node server.js
   ```

4. Load the extension in Chrome (`chrome://extensions`) from this folder

5. Test by selecting code on any webpage and using the extension

### Testing with Different Models

Edit `.env` and change `OPENROUTER_MODEL`:
```bash
OPENROUTER_MODEL=meta-llama/llama-2-70b-chat
OPENROUTER_MODEL=anthropic/claude-3-5-sonnet
```

## Troubleshooting

**"Backend server not responding"**
- Check that your Vercel project is deployed and running
- Verify `OPENROUTER_API_KEY` is set in Vercel environment variables
- Check Vercel logs for errors

**"Invalid API Key"**
- Ensure your OpenRouter API key is correct
- In Vercel: Settings → Environment Variables
- Restart the deployment after updating

**"Extension can't connect"**
- Reload the extension in `chrome://extensions`
- Update `DEFAULT_SERVER_URL` in `background.js` with your actual Vercel URL
- Check that your Vercel domain is added to `host_permissions` in `manifest.json`

**Local testing issues**
- Make sure `node server.js` is running
- Check that `http://localhost:3000` is accessible
- Clear Chrome cache: Settings > Privacy > Clear browsing data

## License

MIT

## Support

For issues or questions:
1. Check the Vercel deployment logs
2. Review OpenRouter API documentation
3. Check Chrome extension console (right-click extension → Inspect)

---

**Ready to improve your code with AI? 🚀**
