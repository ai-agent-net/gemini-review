# Gemini AI PR Reviewer Action

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

An automated AI-powered Code Reviewer for your GitHub Pull Requests using Google's Gemini Models via the official `gemini-cli`. This action works out of the box with both **Google AI Studio** (free tier API Keys) and **Google Cloud Vertex AI** (Enterprise Service Accounts).

_(Русская версия ниже ⬇️)_

## Features

- **Plug-and-play**: Automatically handles repository checkout and Node.js setup.
- **Smart Diffing**: Analyzes only the lines of code changed in the PR.
- **Dual Authentication**: Supports both standard `GEMINI_API_KEY` and Google Cloud Service Account JSON (`use_vertexapi_key: true`).
- **Review States**: Can just leave a comment, or enforce Branch Protection rules by explicitly submitting `--approve` or `--request-changes` reviews based on AI's decision.

## Usage

Create a workflow file in your repository: `.github/workflows/ai-review.yml`:

```yaml
name: AI Code Review

on:
  pull_request:
    types: [review_requested]

jobs:
  review:
    # Optional: trigger only if a specific bot/user is requested
    # if: github.event.requested_reviewer.login == 'ai-agent-net'
    runs-on: ubuntu-latest
    steps:
      - name: Gemini AI Review
        uses: Val-d-emar/gemini-review@v1
        with:
          gemini_api_key: ${{ secrets.GEMINI_API_KEY }}
          # Optional: Make the AI approve/reject the PR
          approval_required: "true"
          # Optional: Use a custom PAT to review as a specific user
          github_token: ${{ secrets.AI_AGENT_PAT }}
```

## Inputs

| Name                | Required | Default               | Description                                                                                                                                           |
| ------------------- | -------- | --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `gemini_api_key`    | **Yes**  | `N/A`                 | Your API Key from Google AI Studio. If `use_vertexapi_key` is true, paste your GCP Service Account JSON string here.                                  |
| `use_vertexapi_key` | No       | `'false'`             | Set to `'true'` if using Google Cloud Vertex AI (requires GCP project with billing enabled).                                                          |
| `github_token`      | No       | `${{ github.token }}` | GitHub Token. Provide a Personal Access Token (PAT) if you want the review to come from a specific user account.                                      |
| `approval_required` | No       | `'false'`             | If `'true'`, the Action will use `gh pr review --approve` or `--request-changes`. To approve, the AI must output `"RESUME: APPROVE"` in its response. |
| `review_prompt`     | No       | _(Default prompt)_    | Custom instructions for the Gemini model.                                                                                                             |
| `review_language`   | No       | `English`             | Custom language for the Gemini model                                                                                                                  |
| `comment_title`     | No       | `# 🤖 AI Review`      | A unique title to prepend to the review, used to find old comments for editing                                                                        |

## License

This project is licensed under the[GNU General Public License v3.0 (GPL-3.0)](LICENSE).

---

Автоматизированный ИИ-ревьюер для ваших Pull Requests на GitHub, работающий на базе моделей Google Gemini через официальный `gemini-cli`. Экшен поддерживает как бесплатные API-ключи от **Google AI Studio**, так и Enterprise-авторизацию через сервисные аккаунты **Google Cloud Vertex AI**.

## Возможности

- **Всё включено**: Экшен сам скачивает код (checkout) и настраивает среду Node.js.
- **Умный анализ**: ИИ читает только `git diff` (измененные строки), а не весь проект.
- **Два режима авторизации**: Поддерживает обычный `GEMINI_API_KEY` и JSON ключи сервисных аккаунтов GCP (`use_vertexapi_key: true`).
- **Управление статусом PR**: Может просто оставить комментарий или жестко управлять слиянием, устанавливая статус **Approve** или **Request Changes** на основе решения ИИ.

## Использование

Создайте файл `.github/workflows/ai-review.yml` в вашем проекте:

```yaml
name: AI Code Review

on:
  pull_request:
    types: [review_requested]

jobs:
  review:
    # Запускать только если ревью запросили у конкретного бота
    # if: github.event.requested_reviewer.login == 'ai-agent-net'
    runs-on: ubuntu-latest
    steps:
      - name: Gemini AI Review
        uses: Val-d-emar/gemini-review@v1
        with:
          gemini_api_key: ${{ secrets.GEMINI_API_KEY }}
          # Включаем логику отклонения/принятия PR
          approval_required: "true"
          # Используем токен конкретного аккаунта для публикации
          github_token: ${{ secrets.AI_AGENT_PAT }}
```

## Параметры (Inputs)

| Имя                 | Обязательно | По умолчанию          | Описание                                                                                                                                            |
| ------------------- | ----------- | --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `gemini_api_key`    | **Да**      | `N/A`                 | API-ключ от Google AI Studio. Если включен Vertex AI, сюда нужно передать JSON сервисного аккаунта GCP.                                             |
| `use_vertexapi_key` | Нет         | `'false'`             | Установите `'true'`, если используете Google Cloud Vertex AI.                                                                                       |
| `github_token`      | Нет         | `${{ github.token }}` | GitHub Токен. Передайте сюда свой Personal Access Token (PAT), чтобы ревью публиковалось от имени вашего бота/аккаунта.                             |
| `approval_required` | Нет         | `'false'`             | Если `'true'`, ИИ будет ставить официальный статус Approve или Request Changes. Для аппрува ИИ должен написать `"RESUME: APPROVE"` в тексте ответа. |
| `review_prompt`     | Нет         | _(Базовый промпт)_    | Ваша кастомная инструкция для ИИ (prompt).                                                                                                          |
| `review_language`   | No          | `English`             | Пользовательский язык для модели                                                                                                                    |
| `comment_title`     | No          | `# 🤖 AI Review`      | Уникальный заголовок для добавления к отзыву, используется для поиска старых комментариев для редактирования                                                                      |

## Лицензия

Этот проект распространяется под лицензией [GNU General Public License v3.0 (GPL-3.0)](LICENSE).
