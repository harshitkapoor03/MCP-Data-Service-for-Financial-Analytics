# MCP-Data-Service-for-Financial-Analytics


A Model Context Protocol (MCP) server implementation providing real time and historical financial data analytics through natural language queries. Developed with FastMCP and deployed using Railway, this service integrates CoinGecko and yfinance APIs to deliver comprehensive financial insights with automatic chart generation and IST timezone support.

## Overview

This asynchronous MCP server enables large language model clients to perform advanced financial analysis via natural language queries. The service automatically retrieves data, computes relevant metrics, generates visualizations, and formats responses for conversational interaction.

### Key Features

- Real-time and historical data retrieval for stocks and cryptocurrencies  
- Secure access using Bearer token authentication with RSA key pairs  
- Performance optimizations, including data downsampling and in-memory rendering  
- Automatic IST timezone conversion for Indian market data  
- Chart generation for graphical analysis of price trends  
- Additional tools: vintage photo filter and daily horoscope service  

## Architecture

The system adheres to the Model Context Protocol specification, exposing discoverable tools to MCP clients.

MCP Client  MCP Server  Data Sources (CoinGecko, Yahoo Finance)

Clients issue natural language queries, which the server maps to specific tools. The server fetches data, processes it, and returns formatted text and images.

## Quick Start

### Prerequisites

- Python 3.8 or higher  
- Railway account for deployment  
- MCP-compatible client (e.g., Puchai)  

### Local Setup

1. Clone the repository:  
   ```bash
   git clone https://github.com/harshitkapoor03/MCP-Data-Service-for-Financial-Analytics.git
   cd MCP-Data-Service-for-Financial-Analytics
   ```
2. Install dependencies:  
   ```bash
   pip install -r requirements.txt
   ```
3. Create a `.env` file with the following variables:  
   ```bash
   AUTH_TOKEN=730E024D
   MY_NUMBER=917044607962
   ```
4. Run the server:  
   ```bash
   python mcp-server.py
   ```
   The server will be accessible at `http://localhost:8086`.

### Deployment on Railway

1. Connect the GitHub repository to Railway.  
2. Set environment variables (`AUTH_TOKEN`, `MY_NUMBER`) in the Railway dashboard.  
3. Deploy the application; Railway will provide a public URL.  

## Configuration

### Authentication

The server uses a custom BearerAuthProvider:

```python
from fastmcp.server.auth.providers.bearer import BearerAuthProvider, RSAKeyPair
from mcp.server.auth.provider import AccessToken

class SimpleBearerAuthProvider(BearerAuthProvider):
    def __init__(self, token: str):
        key_pair = RSAKeyPair.generate()
        super().__init__(public_key=key_pair.public_key, jwks_uri=None, issuer=None, audience=None)
        self.token = token

    async def load_access_token(self, token: str) -> AccessToken | None:
        if token == self.token:
            return AccessToken(token=token, client_id="client", scopes=["*"], expires_at=None)
        return None
```

### Connecting an MCP Client

Configure your MCP client (for example, Puchai) to connect to the deployed server:

```json
{
  "servers": {
    "financial-analytics": {
      "url": "https://.up.railway.app/mcp",
      "auth": {
        "type": "bearer",
        "token": "730E024D"
      }
    }
  }
}
```

## Available Tools

**Stock Time Series Analysis**


**Cryptocurrency Time Series Analysis**


**Vintage Photo Filter**


**Daily Horoscope**


## Data Sources

- **yfinance** for real-time and historical stock data  
- **CoinGecko API** for cryptocurrency market data  

## Performance Optimizations

- Data point downsampling for efficient chart rendering  
- In-memory chart generation to eliminate disk I/O  
- Caching of repeated API calls  
- IST timezone conversion for accurate time display  

## File Structure

```
├── mcp-server.py       # Server implementation
├── requirements.txt    # Dependencies
├── env.txt             # Environment variable template
├── README.md           # Project documentation
└── .railway/           # Deployment configuration
```



## License

This project is licensed under the MIT License.

