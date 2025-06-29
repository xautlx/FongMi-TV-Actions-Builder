# FongMi TV Actions Builder

[![Build](https://github.com/xixu-me/FongMi-TV-Actions-Builder/actions/workflows/build.yml/badge.svg)](https://github.com/xixu-me/FongMi-TV-Actions-Builder/actions/workflows/build.yml)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

An automated GitHub Actions workflow that builds and releases signed APKs from the [FongMi/TV](https://github.com/FongMi/TV) repository.

## 🚀 Features

- **Automated Building**: Monitors the FongMi/TV repository for changes and builds APKs automatically
- **Smart Change Detection**: Only builds when new commits are detected, avoiding unnecessary builds
- **Signed APKs**: Automatically signs APKs with your keystore for distribution
- **Scheduled Builds**: Runs every hour to check for updates
- **Manual Triggers**: Support for force builds via workflow dispatch
- **Automatic Releases**: Creates GitHub releases with built APKs
- **Dependency Management**: Automated dependency updates via Dependabot

## 📋 Prerequisites

Before using this builder, you need:

1. **GitHub Repository**: Fork or create a new repository from this template
2. **Android Keystore**: A valid Android keystore for signing APKs
3. **Repository Secrets**: Required secrets configured in your GitHub repository

## 🔧 Setup Instructions

### 1. Fork This Repository

Click the "Fork" button or use this repository as a template.

### 2. Configure Repository Secrets

Go to your repository **Settings → Secrets and variables → Actions** and add the following secrets:

| Secret Name | Description | Required |
|-------------|-------------|----------|
| `KEYSTORE_BASE64` | Your Android keystore file encoded in Base64 | ✅ Yes |
| `KEYSTORE_PASSWORD` | Password for your keystore | ✅ Yes |
| `KEY_ALIAS` | Alias of your signing key | ✅ Yes |
| `KEY_PASSWORD` | Password for your signing key | ✅ Yes |

### 3. Prepare Your Keystore

#### Option A: Use Existing Keystore

If you have an existing Android keystore:

```bash
# Encode your keystore to Base64
base64 -i your-keystore.jks -o keystore-base64.txt
# Copy the contents of keystore-base64.txt to KEYSTORE_BASE64 secret
```

#### Option B: Create New Keystore

If you need to create a new keystore:

```bash
# Generate a new keystore (requires Java/Android SDK)
keytool -genkey -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias my-key-alias

# Then encode it to Base64
base64 -i my-release-key.jks -o keystore-base64.txt
```

### 4. Enable Actions

1. Go to the **Actions** tab in your repository
2. Click "I understand my workflows, go ahead and enable them"
3. The workflow will start running automatically

## 📅 Build Schedule

- **Automatic**: Every hour (configurable in `.github/workflows/build.yml`)
- **Manual**: Via workflow dispatch with optional force build
- **Smart**: Only builds when changes are detected in the source repository

## 🏗️ Build Process

1. **Source Check**: Clones the latest code from FongMi/TV repository
2. **Change Detection**: Compares current commit with last release
3. **Build Decision**: Decides whether to build based on changes
4. **APK Building**: Builds and signs release APKs using Gradle
5. **Release Creation**: Creates GitHub release with APK files

## 📦 Output

Built APKs are automatically uploaded to GitHub Releases with:

- **Tag**: Short commit hash from source repository
- **Release Notes**: Commit information and build details
- **Files**: All APK variants from the build

## 🔧 Customization

### Modify Source Repository

Edit the environment variables in `.github/workflows/build.yml`:

```yaml
env:
  SOURCE_REPO: "FongMi/TV"  # Change to your preferred repository
  SOURCE_BRANCH: "release"  # Change to your preferred branch
```

### Adjust Build Schedule

Modify the cron schedule in `.github/workflows/build.yml`:

```yaml
schedule:
  - cron: "0 */2 * * *"  # Every 2 hours instead of every hour
```

### Add Build Variants

The workflow builds all variants defined in the source project's `build.gradle`. No additional configuration needed.

## 🛡️ Security Considerations

- **Secrets**: Never commit keystore files or passwords to the repository
- **Permissions**: Repository secrets are only accessible to workflow runs
- **Keystore Cleanup**: Keystore files are automatically cleaned up after builds
- **Base64 Encoding**: Keystores are securely transferred using Base64 encoding

## 🐛 Troubleshooting

### Build Failures

1. **Check Secrets**: Ensure all required secrets are properly configured
2. **Keystore Issues**: Verify keystore Base64 encoding and passwords
3. **Source Repository**: Check if the source repository is accessible
4. **Logs**: Review detailed logs in the Actions tab

### No Builds Triggered

1. **Check Schedule**: Verify the cron schedule is correct
2. **Force Build**: Use workflow dispatch with force build enabled
3. **Permissions**: Ensure Actions are enabled in your repository

## 📄 License

This repository is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.

## ⚠️ Disclaimer

This repository is an automated build tool for the [FongMi/TV](https://github.com/FongMi/TV) application. Please note the following important disclaimers:

### 🔒 Security and Responsibility

- **Use at Your Own Risk**: This tool is provided "as-is" without any warranties or guarantees
- **Keystore Security**: You are responsible for the security of your Android keystore and signing credentials
- **Build Verification**: Always verify the integrity and security of built APKs before distribution
- **Secret Management**: Ensure proper handling of repository secrets and sensitive information

### 📱 Application Content

- **Third-Party Application**: This builder creates APKs from the FongMi/TV repository, which is a third-party application
- **Content Responsibility**: The author of this builder is not responsible for the content, functionality, or behavior of the built applications
- **Legal Compliance**: Users are responsible for ensuring compliance with local laws and regulations regarding the use of built applications
- **Distribution Rights**: Verify that you have the right to distribute any APKs created using this tool

### 🛠️ Technical Limitations

- **Build Accuracy**: While efforts are made to ensure accurate builds, the author cannot guarantee the functionality of resulting APKs
- **Source Dependencies**: This tool depends on the availability and stability of the source repository
- **Platform Changes**: GitHub Actions platform changes may affect the build process
- **Android Compatibility**: Built APKs may not be compatible with all Android devices or versions

### 🚫 No Support Guarantee

- **Community Tool**: This is a community-driven tool with no official support guarantees
- **Issue Resolution**: Issue resolution is provided on a best-effort basis
- **Maintenance**: Continued maintenance and updates are not guaranteed

### 📋 Recommendations

- **Test Thoroughly**: Always test built APKs on your devices before wider distribution
- **Regular Updates**: Keep your fork updated with the latest changes
- **Security Audits**: Regularly audit your build process and dependencies
- **Backup**: Maintain backups of your keystore and important configuration

By using this tool, you acknowledge that you have read, understood, and agree to these disclaimers and limitations.
