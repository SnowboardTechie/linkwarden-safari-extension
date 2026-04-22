> ## ⚠️ Archived — Unmaintained
>
> This project is no longer maintained. Distributing a Safari extension requires an active Apple Developer Program membership and routing every build through App Store Connect / TestFlight, and the friction isn't worth it for a project the maintainer no longer uses day-to-day.
>
> **Status:** Builds from source as of the last commit. No App Store or TestFlight release was ever published. No future updates are planned.
>
> **To use it:** clone and build from source per the instructions below. You'll need your own Apple Developer account — Safari won't load the extension without a Developer ID-signed containing app.
>
> **To take it over:** forks are welcome. If you actively maintain one, feel free to publicize it.
>
> ---

# Linkwarden Safari Extension

A Safari Web Extension for macOS and iOS that brings [Linkwarden](https://linkwarden.app) bookmark management to Safari. This is a community-maintained port of the [official Linkwarden browser extension](https://github.com/linkwarden/browser-extension) for Chrome and Firefox.

## 🎯 What is This?

This repository contains a Safari Web Extension that allows you to:
- Save bookmarks to your Linkwarden instance directly from Safari
- Upload screenshots of web pages
- Save all open tabs at once
- Use right-click context menus to quickly save links
- Works on both macOS and iOS Safari

## 📋 Features

- ✅ **Save Links**: Save URLs with title, description, collection, and tags
- ✅ **Screenshots**: Capture and upload screenshots of web pages
- ✅ **Bulk Save**: Save all tabs in the current window
- ✅ **Context Menu**: Right-click any link to save it
- ✅ **Dual Authentication**: API key or username/password
- ✅ **Cross-Platform**: Works on macOS Safari and iOS Safari
- ✅ **Self-Hosted Support**: Works with both cloud and self-hosted Linkwarden instances

## 🚀 Quick Start

### For Users

**Coming Soon:** The extension will be available on the App Store for macOS and iOS.

**From Source:**
1. Clone this repository
2. Follow the [build instructions](#building-from-source) below

### For Developers

See [Linkwarden for Safari/README.md](./Linkwarden%20for%20Safari/README.md) for detailed development information.

## 📁 Project Structure

```
linkwarden-safari-extension/
├── Linkwarden for Safari/          # Safari Xcode project
│   ├── Linkwarden for Safari.xcodeproj/  # Xcode project file
│   ├── Linkwarden for Safari/            # macOS app wrapper
│   ├── Linkwarden for Safari Extension/  # Extension target
│   ├── dist/                             # Built extension files (generated)
│   └── README.md                         # Safari-specific documentation
│
├── linkwarden-official/            # Official extension source (clone separately)
│   ├── src/                         # TypeScript source code
│   ├── chromium/                    # Chrome manifest
│   └── firefox/                     # Firefox manifest
│
├── build-for-safari.sh            # Main build script (applies patches, builds, resets)
├── copy-to-safari.sh               # Helper script to copy built files
├── safari-patches/                 # Safari-specific patches and manifest
│   ├── README.md                    # Patch documentation
│   ├── auth-fetch.patch            # Replace axios with fetch
│   ├── cache-bookmarks.patch        # Bookmarks API guards
│   ├── background-safari.patch     # Bookmarks/omnibox guards
│   └── manifest-safari.json        # Safari-compatible manifest
│
├── README.md                        # This file
├── RELEASE_STRATEGY.md              # Release and distribution strategy
├── REVIEW_AND_NEXT_STEPS.md         # Development status and next steps
└── SAFARI_CONVERSION_STATUS.md      # Conversion progress tracking
```

## 🛠 Building from Source

### Prerequisites

- **macOS** with Xcode installed
- **Apple Developer Account** (for code signing)
- **Node.js** 18.x.x and npm 9.x.x
- **Git**

### Step 1: Clone This Repository

```bash
# Clone this Safari extension repository
git clone https://github.com/bryan-thompsoncodes/linkwarden-safari-extension.git
cd linkwarden-safari-extension
```

### Step 2: Clone Official Extension Repository

The Safari extension is built from the official Linkwarden browser extension source. You need to clone it separately:

```bash
# Clone the official extension repository
git clone https://github.com/linkwarden/browser-extension.git linkwarden-official
```

**Note:** The `linkwarden-official/` folder is not tracked in Git (it's ignored to avoid merge conflicts and keep the repo focused on Safari-specific changes). You must clone it manually.

### Step 3: Build the Extension

**Important:** We use a patch-based approach to keep the official source clean. The build script applies Safari-specific patches, builds, then resets the official repo.

```bash
# Build for Safari (applies patches, builds, then resets official repo)
./build-for-safari.sh
```

This will:
1. Reset the official repo to clean state
2. Apply Safari-specific patches (see `safari-patches/` directory)
3. Copy Safari-compatible manifest
4. Build the TypeScript source code
5. Copy built files to the Safari Xcode project
6. Reset the official repo back to clean state

**Why patches?** This approach keeps `linkwarden-official/` clean and makes it easy to pull updates from upstream without merge conflicts. See [safari-patches/README.md](./safari-patches/README.md) for details.

### Step 4: Open in Xcode

```bash
open "Linkwarden for Safari/Linkwarden for Safari.xcodeproj"
```

### Step 5: Configure and Build

**Note:** If Xcode shows missing file errors, that's normal - they'll be resolved after running `copy-to-safari.sh` in Step 3.

1. **Set Bundle Identifier:**
   - Select the project in Xcode
   - Go to "Signing & Capabilities"
   - The bundle identifier is currently set to `com.snowboardtechie.linkwarden-for-safari`
   - Change it to your own (e.g., `com.yourname.linkwarden-safari`) or keep the default
   - Select your Apple Developer team

2. **Build and Run:**
   - Press ⌘R to build and run
   - The extension will launch in Safari

3. **Enable Extension:**
   - Safari → Settings → Extensions
   - Enable "Linkwarden for Safari Extension"
   - Enable "Show features for web developers" in Safari → Settings → Advanced (if needed)

## 📖 Documentation

- **[Linkwarden for Safari/README.md](./Linkwarden%20for%20Safari/README.md)** - Detailed usage and development guide
- **[RELEASE_STRATEGY.md](./RELEASE_STRATEGY.md)** - Release and distribution approach
- **[REVIEW_AND_NEXT_STEPS.md](./REVIEW_AND_NEXT_STEPS.md)** - Current status and roadmap
- **[SAFARI_CONVERSION_STATUS.md](./SAFARI_CONVERSION_STATUS.md)** - Conversion progress

## ⚠️ Known Limitations

These features from the Chrome/Firefox version are not available in Safari:

- **Bookmarks API**: Bookmark syncing feature is disabled (Safari doesn't support `browser.bookmarks`)
- **Omnibox**: Search via address bar keyword "lk" is disabled (Safari doesn't support omnibox)

All other features work identically to the Chrome/Firefox versions.

## 🔄 Updating from Official Extension

When the official Linkwarden extension is updated:

```bash
# Update the official extension
cd linkwarden-official
git pull origin main
cd ..

# Build for Safari (patches are applied automatically)
./build-for-safari.sh

# Test in Safari, then commit any Safari-specific changes
git add "Linkwarden for Safari/"
git commit -m "Update from official extension vX.X.X"
```

**Note:** If patches fail to apply after an upstream update, you may need to regenerate them. See [safari-patches/README.md](./safari-patches/README.md) for instructions.

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Test thoroughly on both macOS and iOS Safari
5. Commit your changes (`git commit -m 'Add amazing feature'`)
6. Push to the branch (`git push origin feature/amazing-feature`)
7. Open a Pull Request

### Development Guidelines

- Test on both macOS Safari and iOS Safari
- Ensure compatibility with both self-hosted and cloud Linkwarden instances
- Follow the existing code style
- Update documentation as needed
- Keep Safari-specific changes minimal and well-documented

## 🐛 Troubleshooting

### Extension Not Appearing in Safari

1. Enable Developer mode: Safari → Settings → Advanced → "Show features for web developers"
2. Check Extensions: Safari → Settings → Extensions → Enable "Linkwarden for Safari Extension"
3. Restart Safari completely
4. Rebuild from Xcode

### Build Errors

1. Clean build folder in Xcode (Shift+⌘+K)
2. Ensure `linkwarden-official` is cloned and built
3. Run `./copy-to-safari.sh` to copy files
4. Check bundle identifiers match between app and extension targets

See [Linkwarden for Safari/README.md](./Linkwarden%20for%20Safari/README.md) for more detailed troubleshooting.

## 📝 License

MIT License - See [LICENSE](./LICENSE) file.

This project is based on the [official Linkwarden browser extension](https://github.com/linkwarden/browser-extension), which is also licensed under MIT.

## 🙏 Credits

- **Linkwarden** - The amazing bookmark management system
- **Official Extension** - [linkwarden/browser-extension](https://github.com/linkwarden/browser-extension)
- **Community** - All contributors and users

## 🔗 Links

- [Linkwarden Website](https://linkwarden.app)
- [Linkwarden GitHub](https://github.com/linkwarden/linkwarden)
- [Official Browser Extension](https://github.com/linkwarden/browser-extension)
- [Safari Web Extensions Documentation](https://developer.apple.com/documentation/safariservices/safari_web_extensions)

## 📊 Status

- ✅ Core functionality working
- ✅ macOS Safari support
- ⏳ iOS Safari testing in progress
- ⏳ App Store submission pending

See [REVIEW_AND_NEXT_STEPS.md](./REVIEW_AND_NEXT_STEPS.md) for detailed status.

## 💬 Support

- **Issues**: Open an issue in [this repository](https://github.com/bryan-thompsoncodes/linkwarden-safari-extension) for Safari-specific problems
- **General Linkwarden Issues**: See the [main Linkwarden repository](https://github.com/linkwarden/linkwarden)
- **Feature Requests**: Use the [official repository](https://github.com/linkwarden/linkwarden/issues) with "[Browser Extension]" prefix

---

**Note:** This is a community-maintained project. It is not officially supported by the Linkwarden team, though we strive to maintain compatibility with the official extension.

