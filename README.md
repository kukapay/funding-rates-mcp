# Funding Rates MCP

An MCP server that provides real-time funding rate data across major crypto exchanges, enabling agents to detect arbitrage opportunities. 

[![Discord](https://img.shields.io/discord/1353556181251133481?cacheSeconds=3600)](https://discord.gg/aRnuu2eJ)
![GitHub License](https://img.shields.io/github/license/kukapay/funding-rates-mcp)
![Python Version](https://img.shields.io/badge/python-3.10+-blue)
![Status](https://img.shields.io/badge/status-active-brightgreen.svg)

## Features

- **Real-Time Funding Rates**: Fetches current funding across Binance, OKX, Bybit, Bitget, Gate and CoinEx.
- **Pivoted Table Output**: Displays symbols as rows, exchanges as columns, and includes a `Divergence` column for max funding rate difference.
- **Claude Desktop Integration**: Runs as an MCP server for interactive queries.

## Hosted deployment

A hosted deployment is available on [Fronteir AI](https://fronteir.ai/mcp/kukapay-funding-rates-mcp).

## Installation

### Prerequisites
- Python 3.10+
- [uv](https://github.com/astral-sh/uv) for dependency management
- [Claude Desktop](https://www.anthropic.com/claude) (optional, for interactive queries)
- Git

### Steps
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/kukapay/funding-rates-mcp.git
   cd funding-rates-mcp
   ```

2. **Set Up Dependencies with uv**:
   ```bash
   uv sync
   ```

3. **Configuring Claude Desktop**：

    To use with Claude Desktop:
    1. Edit the Claude configuration file:
       - **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
       - **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`
    2. Add the MCP server:
       ```json
       {
         "mcpServers": {
           "funding-rates-mcp": {
             "command": "uv",
             "args": ["--directory", "/absolute/path/to/funding-rates-mcp", "run", "funding-rates-mcp"]
           }
         }
       }
       ```
    3. Restart Claude Desktop. Look for the hammer icon to confirm integration.

## Usage
    
### Tools

The server provides the following MCP tool for querying funding rates:

- **`compare_funding_rates`**:
  - **Description**: Compares current funding rates for multiple symbols across specified exchanges, returning a pivoted Markdown table with a `Divergence` column showing the maximum funding rate difference for each symbol.
  - **Parameters**:
    - `symbols` (List[str]): List of trading pairs (e.g., `["BTC/USDT:USDT", "ETH/USDT:USDT"]`). Required.
    - `exchanges` (List[str], optional): List of exchanges to compare (e.g., `["binance", "okx"]`). Defaults to all supported exchanges (`binance`, `okx`, `bybit`, `bitget`, `gate`, `coinex`).
    - `params` (Dict, optional): Additional parameters for the API call. Defaults to `{}`.
  - **Returns**: A Markdown table with symbols as rows, exchanges as columns, funding rates as values (formatted as percentages), and a `Divergence` column.
  - **Example**: See the Examples section below.

### Prompts

The server includes the following MCP prompt for generating natural language queries:

- **`compare_funding_rates_prompt`**:
  - **Description**: Generates a natural language prompt for comparing funding rates across all supported exchanges, suitable for Claude Desktop input.
  - **Parameters**:
    - `symbols` (List[str]): List of trading pairs (e.g., `["BTC/USDT:USDT", "ETH/USDT:USDT"]`). Required.
  - **Returns**: A string prompt (e.g., "Compare the funding rates for BTC/USDT:USDT, ETH/USDT:USDT across binance, okx, bybit, bitget, gate, coinex.").
  - **Usage**: Used internally by Claude Desktop to interpret user queries or for testing in MCP Inspector.

### Examples
#### Compare Funding Rates
- **Input** (in Claude Desktop):  
  "Compare funding rates for BTC/USDT:USDT and ETH/USDT:USDT across all exchanges."
- **Output**:
  ```
  Current funding rates:

  | Symbol\Exchange | binance   | okx       | bybit     | bitget    | gate      | coinex    | Divergence |
  |-----------------|-----------|-----------|-----------|-----------|-----------|-----------|------------|
  | BTC/USDT:USDT   | 0.005161% | 0.007548% | 0.010000% | 0.002900% | -0.019200% | -0.035013% | 0.045013%  |
  | ETH/USDT:USDT   | 0.010000% | 0.005839% | 0.010000% | 0.010000% | -0.008200% | 0.000000%  | 0.018200%  |
  ```

#### Compare Specific Exchanges
- **Input** (in Claude Desktop):  
  "Compare funding rates for SOL/USDT:USDT on Binance, OKX, and Bybit."
- **Output**:
  ```
  Current funding rates:

  | Symbol\Exchange | binance   | okx       | bybit     | Divergence |
  |-----------------|-----------|-----------|-----------|------------|
  | SOL/USDT:USDT   | -0.015312% | -0.003639% | -0.004467% | 0.011673%  |
  ```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.


