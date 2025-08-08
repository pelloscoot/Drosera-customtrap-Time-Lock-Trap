# 🚀 Drosera-customtrap-Time-Lock-Trap

**Contract Address:**  
`0x1bA094B65a0b336cb06D49A3A6FC1A2E30f5D059`

**Functionality:**  
🎯 Purpose of This Logic
Goal	Explanation
🧪 Simulate Uncertainty	Produces a result that changes depending on time
⚔️ Test Conditional Logic	Allows your Trap contract to respond to unpredictable behavior
🐞 Debugging	Helps you verify how your trap behaves with dynamic target states

---

## 🖥️ System Requirements

- 2 CPU Cores  
- 4 GB RAM  
- 100 GB NVME  
- OS: Ubuntu 24.04 LTS

---

## ✅ Tested On

🟢 Ubuntu 24.04

---

## 🌐 Get ETH for Hoodi Network

Use the faucet:  
[https://hoodi-faucet.pk910.de/](https://hoodi-faucet.pk910.de/)

---

## ⚙️ Setup Instructions

### 1. Start a screen session

```bash
screen -R drosera
```

### 2. Install dependencies

```bash
sudo apt-get update && sudo apt-get upgrade -y && sudo apt install curl ufw iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y
```

```bash
sudo apt-get install ca-certificates curl gnupg && sudo install -m 0755 -d /etc/apt/keyrings && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg && sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

```bash
sudo apt update -y && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo docker run hello-world
```

---

### 3. Install Drosera CLI

```bash
curl -L https://app.drosera.io/install | bash
source ~/.bashrc
droseraup
```

### 4. Install Foundry

```bash
curl -L https://foundry.paradigm.xyz | bash
source ~/.bashrc
foundryup
```

---

### 5. Init Drosera Trap Project

```bash
curl -fsSL https://bun.sh/install | bash
mkdir my-drosera-trap && cd my-drosera-trap
forge init -t drosera-network/trap-foundry-template
```

```bash
apt install snapd
snap install bun-js
```

```bash
bun install
forge build
```

---

### 6. Configure `drosera.toml`

```toml
path = "out/Trap.sol/Trap.json"
response_contract = "0x1bA094B65a0b336cb06D49A3A6FC1A2E30f5D059"
response_function = "CommunityVoting()"
```

---

### 7. Edit Trap Logic

File: `src/Trap.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import {ITrap} from "drosera-contracts/interfaces/ITrap.sol";

interface IEthereumHoodie {
    function isActive() external view returns (bool);
}

contract Trap is ITrap {
    address public constant RESPONSE_CONTRACT = 0x1bA094B65a0b336cb06D49A3A6FC1A2E30f5D059;
    string constant discordName = "yourdiscord";

    function collect() external view returns (bytes memory) {
        bool active = IEthereumHoodie(RESPONSE_CONTRACT).isActive();
        return abi.encode(active, discordName);
    }

    function shouldRespond(bytes[] calldata data) external pure returns (bool, bytes memory) {
        (bool active, string memory name) = abi.decode(data[0], (bool, string));
        if (!active || bytes(name).length == 0) {
            return (false, bytes(""));
        }
        return (true, abi.encode(name));
    }
}
```

---

### 8. Compile and Deploy

```bash
forge build
DROSERA_PRIVATE_KEY=YOUR_PRIVATE_KEY drosera apply
drosera dryrun
```

---

### ✅ Done
