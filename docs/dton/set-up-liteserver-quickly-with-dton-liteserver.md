# Setting Up a Liteserver Node for The Open Network (TON)

A Liteserver node in TON enables lightweight clients to interact with the blockchain without the need to run a full node. You can set up a Liteserver manually or utilize services like dTON for a more streamlined process.

---

## Traditional Setup

Setting up a Liteserver manually involves several steps, including installing MyTonCtrl, downloading the blockchain state, and configuring the server.

Additionally, you’ll need to ensure your hardware meets the following requirements as of this time of writing:

- **CPU**: At least 16 cores
- **RAM**: Minimum of 128 GB
- **Storage**: At least 1 TB NVMe SSD or provisioned storage with 64k+ IOPS
- **Network**: 1 Gbit/s connectivity
- **Traffic**: 16 TB/month at peak load
- **IP Address**: Public, fixed IP address

The installation process also includes configuring the Liteserver, opening necessary ports on your firewall, and monitoring the node's status.

While the manual setup offers full control over the Liteserver, it is resource-intensive and may feel cumbersome.

For detailed instructions, refer to the [official TON documentation](https://docs.ton.org/v3/guidelines/nodes/running-nodes/liteserver-node).

## Simplified Setup with dTON

Using dTON significantly simplifies the process of setting up a Liteserver node. With dTON, you don't need to worry about the complexities of managing hardware, configuring servers, or dealing with long setup times.

Here is how to set up a Liteserver with dTON in just three simple steps:

### Steps to Set Up a Liteserver with dTON

1. **Access dTON's Web Application**  
   Message to Telegram Bot [@dtontech_bot](https://t.me/dtontech_bot)

2. **Choose a Plan**  
   Select and purchase a Liteserver plan based on your requirements for resources and usage.

3. **Receive Configuration Details**  
   Once your Liteserver is ready, you will receive the necessary details, including:
   - **IP Address** (incl. Int format)
   - **Port**
   - **Public Key** (Base64 and Hex)

You can check them anytime at API Keys tab in web app and check rps stats and dashboard for each of your keys.

dTON removes the need for substantial hardware and technical configurations, allowing you to deploy your Liteserver quickly and efficiently.

---

## Connecting to Your Liteserver Using lite-client

Once your Liteserver is set up via dTON, you can easily connect to it using the `lite-client` tool.

### Steps to Connect

You can use `ton-lite-client` (JS library) to connect to your dTON liteserver

Example:
```
import { LiteClient, LiteRoundRobinEngine, LiteSingleEngine, LiteEngine } from "ton-lite-client";
import { Address } from "@ton/core";

function intToIP(int: number) {
    var part1 = int & 255;
    var part2 = ((int >> 8) & 255);
    var part3 = ((int >> 16) & 255);
    var part4 = ((int >> 24) & 255);

    return part4 + "." + part3 + "." + part2 + "." + part1;
}

let server = {
    "ip": YOUR_IP_INT,
    "port": YOUR_PORT,
    "id": {
        "@type": "pub.ed25519",
        "key": YOUR_BASE64_KEY
    }
}

async function main() {
    const engines: LiteEngine[] = [];
    engines.push(new LiteSingleEngine({
        host: `tcp://${intToIP(server.ip)}:${server.port}`,
        publicKey: Buffer.from(server.id.key, 'base64'),
    }));
    const engine: LiteEngine = new LiteRoundRobinEngine(engines);
    const client = new LiteClient({ engine });
    console.log('get master info')
    const master = await client.getMasterchainInfo()
    console.log('master', master)

    const address = Address.parse('kQC2sf_Hy34aMM7n9f9_V-ThHDehjH71LWBETy_JrTirPIHa');
    const accountState = await client.getAccountState(address, master.last)
    console.log('Account state:', accountState)
}

main()
```

Congrats, now you can interact with your own liteserver that's set up using dTon Liteserver service in just three simple steps.

For questions and support, please message [@dtontech](https://t.me/dtontech) on Telegram