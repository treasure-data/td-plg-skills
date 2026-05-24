# Ask Data Source

Use the AskUserQuestion tool with:

- **Question**: "Which data source should we work with?"
- **Single-select** (not multi-select)
- **Options**:
  | Label | Description |
  |-------|-------------|
  | Synthetic data | Use pre-loaded sample datasets to explore features |
  | Upload my own data | Import CSV, JSON, or other files |

## Conditional behavior

- **Synthetic data**: Use the `treasurebikes` database. Explain that this is a sample dataset with customer, order, and product data that the user can explore.
- **Upload my own data**: Remind the user to upload data using the "+" icon in the chat window. Wait for the file attachment before proceeding with any analysis. If the user already has data uploaded, use that instead. Please avoid using personal or confidential data.
