# TaxHacker

## What This Is

This is part of the Swift Tech Co. working stack for product and engineering. It gathers the TaxHacker notes, examples and fixes that keep delivery fast, and doubles as the reference map we hand to whoever picks up this thread next.

## Images

[![TaxHacker](images/preview.png)](https://github.com/Swift-Tech-Co/TaxHacker)

## ✨ Features

### `1` Analyze photos and invoices with AI

[![Currency Conversion](public/landing/ai-scanner-big.webp)](https://github.com/Swift-Tech-Co/TaxHacker)

Snap a photo of any receipt or upload an invoice PDF, and TaxHacker will automatically recognize, extract, categorize, and store all the information in a structured database.

- **Upload and organize your docs**: Store multiple documents in "unsorted" until you're ready to process them manually or with AI assistance
- **AI data extraction**: Use AI to automatically pull key information like dates, amounts, vendors, and line items
- **Auto-categorization**: Transactions are automatically sorted into relevant categories based on their content
- **Item splitting**: Extract individual items from invoices and split them into separate transactions when needed
- **Structured storage**: Everything gets saved in an organized database for easy filtering and retrieval
- **Choose your LLM**: You can use OpenAI, Google Gemini, or Mistral or even your local LLM (in the self-hosted version). Only you're responsible for the quality and privacy of your data.

TaxHacker works with a wide variety of documents, including store receipts, restaurant bills, invoices, bank statements, letters, even handwritten receipts. It handles any language and any currency with ease.

### `2` Multi-currency support with automatic conversion (even crypto!)

[![Currency Conversion](public/landing/multi-currency.webp)](https://github.com/Swift-Tech-Co/TaxHacker)

TaxHacker automatically detects currencies in your documents and converts them to your base currency using historical exchange rates.

- **Foreight currency detection**: Automatically identify the currency used in any document
- **Historical rates**: Get conversion rates from the actual transaction date
- **All-world coverage**: Support for 170+ world currencies and 14 popular cryptocurrencies (BTC, ETH, LTC, DOT, and more)
- **Flexible input**: Manual entry is always available when you need more control

### `3` Use your own LLM: Ollama, LM Studio, vLLM, LocalAI etc

It's compatible with your local LLM OpenAI-compatible API endpoint. Just make sure that your local model is good in OCR tasks, results are not guaranteed :)

[![Local LLMs](public/landing/local-llms.webp)](https://github.com/Swift-Tech-Co/TaxHacker)

### `4` Organize your transactions using fully customizable categories, projects and fields

[![Transactions Table](public/landing/transactions-big.webp)](https://github.com/Swift-Tech-Co/TaxHacker)

Adapt TaxHacker to your unique needs with unlimited customization options. Create custom fields, projects, and categories that better suit your specific needs, idustry standards or country.

- **Custom categories and projecst**: Create your own categories and projects to group your transactions in any convenient way
- **Custom fields**: You can create unlimited number of custom fields to extraxt more information from your invoices (it's like creating extra columns in Excel)
- **Full-text search**: Search through the actual content of recognized documents
- **Advanced filtering**: Find exactly what you need with search and filter options
- **AI-powered extraction**: Write your own prompts to extract any custom information from documents
- **Bulk operations**: Process multiple documents or transactions at once

### `5` Customize any LLM prompt. Even system ones

[![Custom Categories](public/landing/custom-llm.webp)](https://github.com/Swift-Tech-Co/TaxHacker)

Take full control of how TaxHacker's AI processes your documents. Write custom AI prompts for fields, categories, and projects, or modify the built-in ones to match your specific needs.

- **Customizable system prompts**: Modify the general prompt template in settings to suit your business
- **Field or project-specific prompts**: Create custom extraction rules for your industry-specific documents
- **Full control**: Adjust field extraction priorities and naming conventions to match your workflow
- **Industry optimization**: Fine-tune the AI to understand your specific type of business documents
- **Full transparency**: Every aspect of the AI extraction process is under your control and can be changed right in settings

TaxHacker is 100% adaptable and tunable to your unique requirements  whether you need to extract emails, addresses, project codes, or any other custom information from your documents.

### `6` Flexible data filtering and export

[![Data Export](public/landing/export.webp)](https://github.com/Swift-Tech-Co/TaxHacker)

Once your documents are processed, easily view, filter, and export your complete transaction history exactly how you need it.

- **Advanced filtering**: Filter by date ranges, categories, projects, amounts, and any custom fields
- **Flexible exports**: Export filtered transactions to CSV with all attached documents included
- **Tax-ready reports**: Generate comprehensive reports for your accountant or tax advisor
- **Data portability**: Download complete data archives to migrate to other servicesyour data stays yours

### `7` Self-hosted mode for data privacy

[![Self-hosting](docs/screenshots/exported_archive.png)](https://github.com/Swift-Tech-Co/TaxHacker)

Keep complete control over your financial data with local storage and self-hosting options. TaxHacker respects your privacy and gives you full ownership of your information.

- **Home server ready**: Host on your own infrastructure for maximum privacy and control
- **Docker native**: Simple setup with provided Docker containers and compose files
- **Data ownership**: Your financial documents never leaves your control
- **No vendor lock-in**: Export everything and migrate whenever you want
- **Transparent operations**: Full access to source code and complete operational transparency

## 🛳 Deployment and Self-hosting

TaxHacker can be easily self-hosted on your own infrastructure for complete control over your data and application environment. We provide a [Docker image](./Dockerfile) and [Docker Compose](./docker-compose.yml) setup that makes deployment simple:

```bash
curl -O https://github.com/Swift-Tech-Co/TaxHacker

docker compose up
```

The Docker Compose setup includes:

- TaxHacker application container
- PostgreSQL 17+ database (or connect to your existing database)
- Automatic database migrations on startup
- Volume mounts for persistent data storage
- Production-ready configuration
- `no-new-privileges` security option on all containers

New Docker images are automatically built and published with every release. You can use specific version tags (e.g., `v1.0.0`) or `latest` for the most recent version.

For advanced setups, you can customize the Docker Compose configuration to fit your infrastructure. The default configuration uses the pre-built image from GitHub Container Registry, but you can also build locally using the provided [Dockerfile](./Dockerfile).

Example custom configuration:

```yaml
services:
  app:
    image: ghcr.io/vas3k/taxhacker:latest
    ports:
      - "7331:7331"
    environment:
      - SELF_HOSTED_MODE=true
      - UPLOAD_PATH=/app/data/uploads
      - DATABASE_URL=postgresql://postgres:postgres@localhost:5432/taxhacker
    volumes:
      - ./data:/app/data
    restart: unless-stopped
```

### Environment Variables

Configure TaxHacker for your specific needs with these environment variables:

| Variable | Required | Description | Example |
|----------|----------|-------------|---------|
| `UPLOAD_PATH` | Yes | Local directory for file uploads and storage | `./data/uploads` |
| `PORT` | No | Port to run the application on | `7331` (default) |
| `BASE_URL` | No | Base URL for the application | `https://github.com/Swift-Tech-Co/TaxHacker |
| `SELF_HOSTED_MODE` | No | Set to "true" for self-hosting: enables auto-login, custom API keys, and additional features | `true` |
| `DISABLE_SIGNUP` | No | Disable new user registration on your instance | `false` |
| `BETTER_AUTH_SECRET` | Recommended | Secret key for authentication and email credential encryption (minimum 16 characters). In self-hosted Docker, TaxHacker auto-generates and persists one in `./data/.better_auth_secret` if unset. | `your-secure-random-key` |
| `POSTGRES_PASSWORD` | Yes (`docker-compose.build.yml`) | Password for the bundled Postgres when building locally  used in both `POSTGRES_PASSWORD` and `DATABASE_URL` | output of `openssl rand -hex 24` |
| `POSTGRES_USER` | No (`docker-compose.build.yml`) | Database user (defaults to `postgres`) | `postgres` |
| `POSTGRES_DB` | No (`docker-compose.build.yml`) | Database name (defaults to `taxhacker`) | `taxhacker` |

> 💡 **Need internet access with authentication?** See [docs/self-hosted-public-access.md](docs/self-hosted-public-access.md) for a workaround (yes, it's cursed).

## ⌨️ Local Development

We use:

- **Next.js 15+** for the frontend and API
- **Prisma** for database models and migrations
- **PostgreSQL** as the database (PostgreSQL 17+ recommended)
- **Ghostscript and GraphicsMagick** for PDF processing (install on macOS via `brew install gs graphicsmagick`)

Set up your local development environment:

```bash
# Clone the repository
git clone https://github.com/Swift-Tech-Co/TaxHacker
cd TaxHacker

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env

# Edit .env with your configuration
# Make sure to set DATABASE_URL to your PostgreSQL connection string
# Example: postgresql://user@localhost:5432/taxhacker

# Initialize the database
npx prisma generate && npx prisma migrate dev

# Start the development server
npm run dev
```

Visit `https://github.com/Swift-Tech-Co/TaxHacker to see your local TaxHacker instance in action.

For a production build, instead of `npm run dev` use the following commands:

```bash
# Build the application
npm run build

# Start the production server
npm run start
```

## 🤝 Contributing

No AI-slop PRs. Please open a new Issue and discuss the details with maintainers before sending new changes.

## 📄 License

TaxHacker is licensed under the [MIT License](LICENSE).

---

## Swift Tech Co.

Built and maintained by Swift Tech Co.

AI Automation | FinTech Platforms | Custom Software and Mobile Apps | SaaS Platforms | Cloud Infrastructure | Cybersecurity | SEO and Marketing | API Integration

**Join our community on Telegram:** [t.me/+Qt_eFvkb2rliMzNh](https://t.me/+Qt_eFvkb2rliMzNh)

Home | Services | Careers | Referral Program | Influencers | About | Contact

**Connect**

- Website: https://github.com/Swift-Tech-Co/TaxHacker
- LinkedIn: [linkedin.com/in/stephenoladoke](https://www.linkedin.com/in/stephenoladoke/)
- LinkedIn (Company): [linkedin.com/company/swift-tech-co](https://www.linkedin.com/company/swift-tech-co)
- X: [@MrOladoke](https://x.com/MrOladoke)
- Instagram: [@mr.oladoke](https://www.instagram.com/mr.oladoke/)
- Organization: [github.com/Swift-Tech-Co](https://github.com/Swift-Tech-Co)

## Sources

Compiled and maintained from upstream [vas3k/TaxHacker](https://github.com/vas3k/TaxHacker) under permissive open-source license.

---

Maintained by [Swift Tech Co.](https://github.com/Swift-Tech-Co) - [Telegram](https://t.me/+Qt_eFvkb2rliMzNh)

## Sources

Compiled and maintained from upstream [vas3k/TaxHacker](https://github.com/vas3k/TaxHacker) under permissive open-source license.

---

Maintained by [Swift Tech Co.](https://github.com/Swift-Tech-Co) - [Telegram](https://t.me/+Qt_eFvkb2rliMzNh)

## Sources

Compiled and maintained from upstream [vas3k/TaxHacker](https://github.com/vas3k/TaxHacker) under permissive open-source license.

---

Maintained by [Swift Tech Co.](https://github.com/Swift-Tech-Co) - [Telegram](https://t.me/+Qt_eFvkb2rliMzNh)

## Sources

Compiled and maintained from upstream [vas3k/TaxHacker](https://github.com/vas3k/TaxHacker) under permissive open-source license.

---

Maintained by [Swift Tech Co.](https://github.com/Swift-Tech-Co) - [Telegram](https://t.me/+Qt_eFvkb2rliMzNh)

## Sources

Compiled and maintained from upstream [vas3k/TaxHacker](https://github.com/vas3k/TaxHacker) under permissive open-source license.

---

Maintained by [Swift Tech Co.](https://github.com/Swift-Tech-Co) - [Telegram](https://t.me/+Qt_eFvkb2rliMzNh)