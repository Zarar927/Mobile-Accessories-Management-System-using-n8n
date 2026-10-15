# 🤖 AI-Powered Inventory Management Assistant

An intelligent inventory management automation built with **n8n, AI Agents, Airtable, and OpenRouter**. This project allows users to manage inventory through natural-language chat commands, automatically update stock quantities, and modify product prices when explicitly requested.

Instead of manually searching through inventory records and editing quantities, users can communicate with an AI assistant that interprets their instructions and interacts with Airtable.

## ✨ Features

- **Natural-Language Inventory Management:** Add stock, record sales, and update inventory using conversational instructions.
- **AI-Powered Decision Making:** Uses an AI Agent powered by the DeepSeek V4 Flash model through OpenRouter.
- **Airtable Integration:** Searches inventory records and updates quantities and unit prices.
- **Automatic Stock Calculations:** Decreases stock for sales and increases stock for explicitly stated restocking.
- **Stock Validation:** Prevents the assistant from intentionally reducing inventory below zero.
- **Price Management:** Updates the unit price only when the user explicitly provides a new price.
- **Conversation Memory:** Uses n8n Simple Memory to maintain conversational context within its configured memory window.
- **Tool-Based Automation:** Connects the AI Agent to dedicated Airtable search, stock-update, and price-update tools.

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| n8n | Workflow automation and orchestration |
| AI Agent | Interprets user instructions and coordinates tools |
| OpenRouter | Provides access to the language model |
| DeepSeek V4 Flash | Natural-language understanding and reasoning |
| Airtable | Stores and manages inventory records |
| Simple Memory | Maintains recent conversational context |

## 🏗️ Workflow Architecture

The workflow follows this process:

1. **Chat Trigger:** Receives an inventory instruction from the user.
2. **AI Agent:** Interprets the request and determines the required action.
3. **OpenRouter Chat Model:** Processes the request using the configured language model.
4. **Simple Memory:** Supplies recent conversation context to the AI Agent.
5. **Search Records in Airtable:** Finds the relevant inventory item and retrieves its record ID, current quantity, and unit price.
6. **Stock Update Tool:** Updates the item's total stock quantity.
7. **Price Update Tool:** Changes the unit price only when explicitly requested.
8. **Chat Response:** Confirms the action and reports the resulting inventory values.

## 💬 Example Commands

The following examples illustrate the intended behavior of the assistant.

| User instruction | Expected action |
|---|---|
| "I sold 2 office chairs." | Decrease chair stock by 2 |
| "A customer bought 5 keyboards." | Decrease keyboard stock by 5 |
| "Add 10 office chairs to stock." | Increase chair stock by 10 |
| "We received 20 new keyboards." | Increase keyboard stock by 20 |
| "Update the office chair price to 5,000 each." | Update the chair's unit price |
| "I sold 100 chairs." | Refuse the update if sufficient stock is unavailable |
| "Update the stock for chairs." | Ask for clarification if the intended change is unclear |

*Actual behavior depends on the Airtable data, model interpretation, tool configuration, and successful execution.*

## 📦 Inventory Management Rules

The AI Agent follows these business rules:

### 1. Stock Decreases

Words such as *sold, bought, purchased,* and *customer bought* are interpreted as items leaving the shop.

**Formula:**

`New Quantity = Current Quantity - Sold Quantity`

### 2. Stock Increases

Stock increases only when the user explicitly indicates that new inventory has arrived or is being added.

**Formula:**

`New Quantity = Current Quantity + Added Quantity`

### 3. Prevent Negative Stock

The assistant checks the current quantity before updating. If a sale exceeds the available stock, it should report insufficient stock and avoid the update.

### 4. Price Updates

The `Value` field represents the unit price. It should remain unchanged unless the user explicitly provides a new price.

### 5. Record Identification

The assistant must search Airtable before modifying an item. It should use the actual record ID returned by the search tool rather than guessing an ID.

### 6. Ambiguous Instructions

If the requested stock movement is unclear, the assistant should ask the user for clarification instead of guessing.

## ⚙️ Prerequisites

Before using this workflow, you need:

- An n8n instance, either self-hosted or hosted.
- An Airtable account with an inventory base and an `Inventory Items` table.
- An OpenRouter account and API access to the selected model.
- Appropriate Airtable OAuth2 credentials configured in n8n.

Your Airtable table should contain the fields used by the workflow, including:

- `Item Name`
- `SKU`
- `Current Quantity`
- `Value`

Additional fields can be maintained in Airtable as needed.

## 🚀 Installation and Setup

### Step 1: Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
cd YOUR_REPOSITORY
```

Replace the repository URL with your actual GitHub repository.

### Step 2: Import the Workflow

1. Open your n8n instance.
2. Create a new workflow or open the workflow import option.
3. Import the exported workflow JSON file from this repository.
4. Review the nodes and connections.

### Step 3: Configure OpenRouter

1. Create or access your OpenRouter account.
2. Generate an API key.
3. In n8n, create an OpenRouter credential.
4. Select the credential in the **OpenRouter Chat Model** node.
5. Confirm that the selected model is available to your account.

### Step 4: Configure Airtable

1. Open the Airtable nodes in the workflow.
2. Configure your Airtable OAuth2 credentials.
3. Select your own Airtable base and inventory table.
4. Confirm that the required fields exist.
5. Test the search tool before testing any updates.

Use your own base and table when importing the workflow into another account.

### Step 5: Test the Workflow

Start with a test inventory record and try:

- Searching for an existing item.
- Increasing its quantity.
- Decreasing its quantity.
- Updating its unit price.
- Attempting a sale that exceeds available stock.
- Providing an ambiguous instruction.

Verify the actual Airtable values after every update.

### Step 6: Activate the Workflow

Once the credentials, tool mappings, and test results are correct, activate the workflow in n8n.

**Important:** The supplied workflow export is inactive by default. It also contains Airtable base and table references, so configure your own resources where appropriate.

## 🔐 Security Best Practices

- Never commit API keys, passwords, OAuth tokens, or private credentials to GitHub.
- Configure credentials using n8n's credential manager.
- Review exported workflow JSON files before publishing them.
- Avoid sharing real inventory data, private customer information, or internal business records.
- Test stock-changing actions on sample records before using the workflow in a live shop.
- Restrict access to the n8n instance and Airtable base to authorized users.

## ⚠️ Limitations

- The assistant relies on the language model to interpret instructions correctly.
- Similar item names may require clarification.
- The workflow does not provide a separate transaction ledger or guaranteed audit trail in the configuration shown.
- Simple Memory provides conversational context; it is not a replacement for persistent inventory storage.
- Tool execution failures, concurrent updates, and external service outages may affect results.
- The workflow's stock validation instructions should be tested carefully before production use.

## 🔮 Future Improvements

Potential enhancements include:

- Low-stock alerts and automatic reorder suggestions.
- Inventory movement history and audit logging.
- Barcode-based inventory operations.
- Sales and stock reports.
- Multi-user authentication and role-based access control.
- Confirmation steps for sensitive stock adjustments.
- Notifications through email, Telegram, or WhatsApp.
- Automated reconciliation and scheduled inventory summaries.

## 🎯 Use Cases

This project can be adapted for:

- Retail shops and small businesses.
- Office supply inventories.
- Electronics and accessories stores.
- Warehouse stock management.
- Small e-commerce inventory operations.

## 👨‍💻 Author

**Zarar Ahmed**

Artificial Intelligence graduate interested in AI agents, workflow automation, machine learning, and practical AI applications.

- GitHub: [Zarar927](https://github.com/Zarar927)
- Portfolio: [zarar-portfolio.netlify.app](https://zarar-portfolio.netlify.app/)

## 📄 License

Choose a license before distributing or reusing this project. For example, the MIT License permits broad reuse when its conditions are followed. Add a `LICENSE` file if you choose MIT.

---

⭐ If you find this project useful, consider starring the repository and contributing improvements.
