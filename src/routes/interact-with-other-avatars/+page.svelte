<!-- Svelte demo code for the "Interact with other avatars" section of the Circles SDK documentation -->
<script lang="ts">
    import {Sdk, type ChainConfig, type SdkContractRunner, Avatar} from "@circles-sdk/sdk";
    import {BrowserProvider} from "ethers";
    import {onMount} from "svelte";

    let avatar: Avatar | undefined;
    let sdk: Sdk | undefined;
    let error: Error | undefined;
    let trustAddress: string = "";
    let transferRecipientAddress: string = "";
    let transferAmount: number = 0;

    const chainConfig: ChainConfig = {
        pathfinderUrl: 'https://pathfinder.aboutcircles.com',
        circlesRpcUrl: "https://rpc.helsinki.aboutcircles.com",
        v1HubAddress: "0x29b9a7fbb8995b2423a71cc17cf9810798f6c543"
    };

    async function getRunner(): Promise<SdkContractRunner> {
        const w: any = window;
        const browserProvider = new BrowserProvider(w.ethereum);
        const signer = await browserProvider.getSigner();
        const address = await signer.getAddress();

        return {
            runner: signer,
            address: address
        };
    }

    async function getSdk() {
        const runner = await getRunner();
        return new Sdk(chainConfig, runner);
    }

    async function handleError<T>(fn: () => Promise<T>): Promise<T | undefined> {
        try {
            return await fn();
        } catch (e) {
            error = <any>e;
            return undefined;
        }
    }

    onMount(async () => {
        sdk = await getSdk();

        await handleError(async () => {
            avatar = await sdk?.getAvatar(sdk?.contractRunner.address);
        });
    });

    async function trust() {
        await handleError(async () => {
            await avatar?.trust(trustAddress);
        });
    }

    async function untrust() {
        await handleError(async () => {
            await avatar?.untrust(trustAddress);
        });
    }

    async function mint() {
        await handleError(async () => {
            await avatar?.personalMint();
        });
    }

    async function transfer() {
        await handleError(async () => {
            await avatar?.transfer(transferRecipientAddress, transferAmount);
        });
    }
</script>
{#if error}
    <p style="color: darkred">
        Error: {error.message}
    </p>
{/if}
{#if !avatar}
    <p>
        Loading avatar...
    </p>
{:else}
    <p>
        Avatar {avatar.avatarInfo?.avatar} is {avatar.avatarInfo?.type}
    </p>
    <p>
        Add/remove trust relations:<br/>
        <input type="text" bind:value={trustAddress} placeholder="0x.."/>
        <button on:click={trust}>Trust</button>
        <button on:click={untrust}>Untrust</button>
    </p>
    <p>
        Mint personal tokens:<br/>
        <button on:click={mint}>Mint</button>
    </p>
    <p>
        Transfer Circles:<br/>
        <input type="text" bind:value={transferRecipientAddress} placeholder="0x.."/>
        <input type="number" bind:value={transferAmount} placeholder="Amount"/>
        <button on:click={transfer}>Transfer</button>
    </p>
{/if}