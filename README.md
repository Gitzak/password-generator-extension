# 🔐 Password Generator Extension

<img width="1400" height="560" alt="1" src="https://github.com/user-attachments/assets/fc11b1be-e17b-4864-8d99-88c28846d31e" />

A simple, fast, and secure browser extension for generating strong random passwords directly from your browser.

Built with **Vue 3**, **TypeScript**, and **Vite**.

---

## ✨ Features

- Generate secure random passwords instantly
- Choose password length
- Include or exclude:
  - Uppercase letters
  - Lowercase letters
  - Numbers
  - Symbols
- Copy generated passwords to the clipboard
- Clean and simple user interface
- Lightweight and fast
- Works directly from the browser toolbar
- No account required
- No password data is stored or sent anywhere

---

## 🛡️ Privacy

This extension works entirely locally in your browser.

It does **not**:

- Collect personal information
- Track browsing activity
- Store generated passwords
- Send generated passwords to external servers
- Use analytics or advertising services

For more information, see the [Privacy Policy](./PRIVACY_POLICY.md).

---

## 🖼️ Preview

<!-- Add your extension screenshots here -->

```text
Password Generator
├── Password Length
├── Uppercase
├── Lowercase
├── Numbers
├── Symbols
├── Generate Password
└── Copy Password
```

---

## 🛠️ Tech Stack

- [Vue 3](https://vuejs.org/)
- [TypeScript](https://www.typescriptlang.org/)
- [Vite](https://vite.dev/)
- Browser Extension APIs

---

## 📦 Installation

### Development

Clone the repository:

```bash
git clone https://github.com/GitZak/password-generator-extension.git
```

Go to the project directory:

```bash
cd password-generator-extension
```

Install dependencies:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

---

## 🏗️ Build

Create the production build:

```bash
npm run build
```

The production files will be generated inside:

```text
dist/
```

---

## 🌐 Install the Extension Manually

For Chrome or Chromium-based browsers:

1. Build the extension:

```bash
npm run build
```

2. Open:

```text
chrome://extensions
```

3. Enable **Developer mode**.

4. Click **Load unpacked**.

5. Select the generated `dist` folder.

The extension should now appear in your browser toolbar.

---

## 📁 Project Structure

```text
password-generator-extension/
│
├── public/
├── src/
├── .gitignore
├── PRIVACY_POLICY.md
├── README.md
├── index.html
├── package.json
├── tsconfig.json
└── vite.config.ts
```

---

## 🔒 Security

Password generation is performed locally inside the browser.

Generated passwords are not transmitted to any remote server.

For sensitive use cases, always make sure you are using the latest version of the extension and your browser.

---

## 🚀 Future Improvements

Possible future features:

- Password strength indicator
- Password history stored locally
- Passphrase generator
- Custom character exclusions
- Generate multiple passwords at once
- Dark / light theme
- Localization
- Firefox support

---

## 👨‍💻 Author

Developed by **Zakaria Belrhali**

GitHub: [@GitZak](https://github.com/GitZak)

---

## 📄 License

This project is currently provided for educational and personal use.

A license can be added later if the project becomes open source.
