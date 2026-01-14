<div align="center">

# GitHub Copilot CLI (Public Preview)

**The power of GitHub Copilot, now in your terminal.**https://docs.github.com/copilot/concepts/agents/about-copilot-cli

![Copilot CLI Splash](https://github.com/user-attachments/assets/51ac25d2-c074-467a-9c88-38a8d76690e3)

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![GitHub Copilot](https://img.shields.io/badge/Powered%20by-GitHub%20Copilot-blueviolet)](https://github.com/features/copilot)

</div>

---

## 📖 Описание

**GitHub Copilot CLI** переносит возможности ИИ-ассистента GitHub Copilot прямо в твою командную строку. Это позволяет создавать, отлаживать и анализировать код с помощью естественного языка, не выходя из терминала.

Инструмент глубоко интегрирован с экосистемой GitHub и поддерживает сложные "агентные" задачи, такие как планирование и выполнение команд после твоего одобрения.

### ✨ Основные возможности
- **Native Terminal Experience:** Работа с ИИ без переключения контекста.
- **Интеграция с GitHub:** Доступ к репозиториям, задачам (Issues) и пул-реквестам.
- **Агентные функции:** Построение планов по исправлению багов и рефакторингу кода.
- **Контроль:** Вы всегда видите и подтверждаете действие перед его выполнением.

---

## 🚀 Установка

Вы можете установить CLI несколькими способами:

### Через NPM (универсальный способ)
```bash
npm install -g @github/copilot
# Сборка из этого репозитория
​Клонируйте проект: git clone https://github.com/ivanm696/copilot-cli.git
​Перейдите в папку: cd copilot-cli
​Установите зависимости: npm install
​Соберите и привяжите: npm run build && npm link
​🔑 Аутентификация
​Для использования требуется активная подписка Copilot.
1.Обычный вход:
Запустите команду и следуйте инструкциям в браузере:
copilot /login
2.Через персональный токен (PAT):
Если обычный вход не работает, создайте Personal Access Token с разрешением "Copilot Requests" и добавьте его в переменные окружения:
export GH_TOKEN="ваш_токен"
# 🛠 Настройка быстрых команд (Aliases)
​Чтобы использовать короткие команды вроде ??, добавьте следующую строку в ваш конфиг оболочки (.bashrc или .zshrc):
eval "$(github-copilot-cli alias -- "$0")"
# Примеры использования:
​?? как разархивировать tar.gz файл? — получить команду терминала.
​git? сделать коммит с описанием правки — помощь с Git.
​gh? список открытых PR — работа с GitHub CLI.
​📂 Игнорирование лишних файлов (.gitignore)
​Для корректной работы рекомендуется создать файл .gitignore со следующим содержимым:
node_modules/
dist/
build/
.env
*.log
# 📢 Обратная связь
​Это версия Public Preview. Если вы нашли ошибку или у вас есть идея, пожалуйста, откройте Issue в этом репозитории или используйте команду /feedback прямо в CLI.
​<div align="center">
<sub>Разработано на основе официального GitHub Copilot CLI</sub>
</div>

## 📦 Getting Started

### Supported Platforms

- **Linux**
- **macOS**
- **Windows**

### Prerequisites

- (On Windows) **PowerShell** v6 or higher
- An **active Copilot subscription**. See [Copilot plans](https://github.com/features/copilot/plans?ref_cta=Copilot+plans+signup&ref_loc=install-copilot-cli&ref_page=docs).

If you have access to GitHub Copilot via your organization or enterprise, you cannot use GitHub Copilot CLI if your organization owner or enterprise administrator has disabled it in the organization or enterprise settings. See [Managing policies and features for GitHub Copilot in your organization](http://docs.github.com/copilot/managing-copilot/managing-github-copilot-in-your-organization/managing-github-copilot-features-in-your-organization/managing-policies-for-copilot-in-your-organization) for more information.

### Installation

Install with [WinGet](https://github.com/microsoft/winget-cli) (Windows):

```bash
winget install GitHub.Copilot
```

```bash
winget install GitHub.Copilot.Prerelease
```

Install with [Homebrew](https://formulae.brew.sh/cask/copilot-cli) (macOS and Linux):

```bash
brew install copilot-cli
```

```bash
brew install copilot-cli@prerelease
```

Install with [npm](https://www.npmjs.com/package/@github/copilot) (macOS, Linux, and Windows):

```bash
npm install -g @github/copilot
```

```bash
npm install -g @github/copilot@prerelease
```

Install with the install script (macOS and Linux):

```bash
curl -fsSL https://gh.io/copilot-install | bash
```

Or

```bash
wget -qO- https://gh.io/copilot-install | bash
```

Use `| sudo bash` to run as root and install to `/usr/local/bin`.

Set `PREFIX` to install to `$PREFIX/bin/` directory. Defaults to `/usr/local`
when run as root or `$HOME/.local` when run as a non-root user.

Set `VERSION` to install a specific version. Defaults to the latest version.

For example, to install version `v0.0.369` to a custom directory:

```bash
curl -fsSL https://gh.io/copilot-install | VERSION="v0.0.369" PREFIX="$HOME/custom" bash
```

### Launching the CLI

```bash
copilot
```

On first launch, you'll be greeted with our adorable animated banner! If you'd like to see this banner again, launch `copilot` with the `--banner` flag.

If you're not currently logged in to GitHub, you'll be prompted to use the `/login` slash command. Enter this command and follow the on-screen instructions to authenticate.

#### Authenticate with a Personal Access Token (PAT)

You can also authenticate using a fine-grained PAT with the "Copilot Requests" permission enabled.

1. Visit https://github.com/settings/personal-access-tokens/new
2. Under "Permissions," click "add permissions" and select "Copilot Requests"
3. Generate your token
4. Add the token to your environment via the environment variable `GH_TOKEN` or `GITHUB_TOKEN` (in order of precedence)

### Using the CLI

Launch `copilot` in a folder that contains code you want to work with.

By default, `copilot` utilizes Claude Sonnet 4.5. Run the `/model` slash command to choose from other available models, including Claude Sonnet 4 and GPT-5.

Each time you submit a prompt to GitHub Copilot CLI, your monthly quota of premium requests is reduced by one. For information about premium requests, see [About premium requests](https://docs.github.com/copilot/managing-copilot/monitoring-usage-and-entitlements/about-premium-requests).

For more information about how to use the GitHub Copilot CLI, see [our official documentation](https://docs.github.com/copilot/concepts/agents/about-copilot-cli).

## 📢 Feedback and Participation

We're excited to have you join us early in the Copilot CLI journey.

This is an early-stage preview, and we're building quickly. Expect frequent updates--please keep your client up to date for the latest features and fixes!

Your insights are invaluable! Open issue in this repo, join Discussions, and run `/feedback` from the CLI to submit a confidential feedback survey!
