---
title: "Sharing data and querying results"
sidebar_position: 2
---

# Client CLI
Client CLI provides the following functionalities:

- Sharing ETH Balance

  You can anonymously share your ETH balance at Binance

- Query Computation

  You can retrieve statistical metrics such as mean, median, max, min, and Gini coefficient based on users’ historical ETH balance contributions

## Installation
There are two ways to install the client CLI:

### 1. From Source Code
This method involves building the client entirely from the source code, offering the most security through full transparency and control.

- Bandwidth requirement: Moderate
- Downloads one repository and dependencies for a Rust executable and a Python3 script. Additionally, it may require downloading Rust, Python3, and Poetry if not already installed.

#### Steps
1. Clone the repository and cd to the repository root:
   ```bash
   git clone https://github.com/MPCStats/mpc-demo-infra.git
   cd mpc-demo-infra
   ```

1. Set up the environment:

   2.1. Setting up client environment:

       *If you have already run `./setup_env.sh` to set up the environment for running the infrastructure servers on the current machine, skip this step. Otherwise, this step will delete `binabce_verifier`:*

       ```bash
       ./setup_env.sh --client
       ```

   2.2. Additional `Client CLI` Configurations:

       - If using HTTPS
         1. Set PARTY_WEB_PROTOCOL=https and update COORDINATION_SERVER_URL to use https in `.env.client_cli` in the repository root.
         1. Rename the private key and certificate files of your domain as `privkey.pem` and `fullchain.pem` respectively and add them to `ssl_certs` directory at the repository root.

       - If using multiple servers, update both COORDINATION_SERVER_URL, PARTY_HOSTS and NOTARY_SERVER_HOST with the appropriate IP addresses in `.env.client_cli` in the repository root.

### 2. Using Pre-Built Binary
*(Pre-built binaries work only with a registered domain)*

This method relies on trusting pre-compiled binary. It is the least secure but the simplest and fastest way to install the client.

- Bandwidth requirement: Low
- Downloads a shell script and two binaries (less than 50MB).

Note: The binaries are built directly from the source code using public GitHub workflows, defined and available in this repository. This ensures participants can verify the process and confirm binary integrity.

#### Steps
1. Download the script to fetch and execute appropriate binary
   ```
   curl -L -o share-data.sh https://github.com/MPCStats/mpc-demo-infra/releases/latest/download/share-data.sh
   ```
1. Create `.env.client_cli' in the current directory with the following content:
   ```bash
   COORDINATION_SERVER_URL=http://127.0.0.1:8005
   PARTY_HOSTS=["127.0.0.1", "127.0.0.1", "127.0.0.1"]
   PARTY_PORTS=[8006,8007,8008]
   PARTY_WEB_PROTOCOL=http
   NOTARY_SERVER_HOST=127.0.0.1
   ```

   - If using HTTPS
     1. Set PARTY_WEB_PROTOCOL=https and update COORDINATION_SERVER_URL to use https in `.env.client_cli` in the repository root.
     1. Rename the private key and certificate files of your domain as `privkey.pem` and `fullchain.pem` respectively and add them to `ssl_certs` directory at the repository root.

   - If using multiple servers, update both COORDINATION_SERVER_URL, PARTY_HOSTS and NOTARY_SERVER_HOST with the appropriate IP addresses in `.env.client_cli` in the repository root.

## Execution

### Sharing ETH Balance
1. Get the Binance API key and secret, following the instructions in [Get Your Binance API Key](https://github.com/MPCStats/mpc-demo-infra/blob/main/mpc_demo_infra/client_cli/docker/README.md#step-1-get-your-binance-api-key)


1. Execute `Client CLI`:

   - Built from the source code

     Make sure that you are at the repository root before proceeding.

     - Single-Server Local Configuration
       1. Execute `Client CLI`:
          ```bash
          poetry run client-share-data <eth-address> <binance-api-key> <binance-api-secret>  --notary-crt-path $(pwd)/notary.crt
          ```
     - Multi-Server Configuration
       1. Follow the instructions in Client CLI Configuration to create a `.env.client_cli` in the repository root directory

       1. Execute `Client CLI`:
          ```bash
          poetry run client-share-data <eth-address> <binance-api-key> <binance-api-secret>
          ```

   - With `share-data.sh` that makes use of pre-built binary
     1. Follow the instructions in Client CLI Configuration to create an appropriate `.env.client_cli` file in the current directory.

     1. Execute `Client CLI` via `share-data` script:
        ```bash
        curl -L -o share-data.sh https://github.com/MPCStats/mpc-demo-infra/releases/latest/download/share-data.sh
        chmod +x share-data.sh
        ./share-data.sh <eth-address> <binance-api-key> <binance-api-secret>
        ```

### Query Computation
Make sure that you are at the repository root before proceeding.

```bash
poetry run client-query
```

