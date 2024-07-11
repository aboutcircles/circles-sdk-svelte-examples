<script lang="ts">
    import {Sdk, type ChainConfig, type SdkContractRunner, Avatar} from "@circles-sdk/sdk";
    import {BrowserProvider} from "ethers";
    import {onMount} from "svelte";

    let avatar: Avatar | undefined;
    let sdk: Sdk | undefined;
    let error: Error | undefined;
    let totalBalance: string | undefined;
    let mintableAmount: string | undefined;
    let trustRelations: any;
    let transactionHistory: any;

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

    onMount(async () => {
        sdk = await getSdk();
    });

    async function loadAvatar() {
        error = undefined;
        try {
            const loadedAvatar = await sdk?.getAvatar(sdk?.contractRunner.address);

            const [balance, mintAmount, trustRels, txnHistory] = await Promise.all([
                loadedAvatar.getTotalBalance(),
                loadedAvatar.getMintableAmount(),
                loadedAvatar.getTrustRelations(),
                loadedAvatar.getTransactionHistory(10)
            ]);

            totalBalance = balance;
            mintableAmount = mintAmount;
            trustRelations = trustRels;
            transactionHistory = txnHistory;
            avatar = loadedAvatar;
        } catch (e) {
            error = e;
        }
    }
</script>

{#if error}
    <p style="color: darkred">
        Error: {error.message}
    </p>
{/if}
{#if !avatar}
    <button on:click={loadAvatar}>
        Load avatar
    </button>
{:else}
    <p>
        Avatar {avatar.avatarInfo.avatar} is {avatar.avatarInfo.type}
    </p>
    <p>
        Circles balance: {totalBalance}
    </p>
    <p>
        Mintable amount: {mintableAmount}
    </p>
    <div>
        Trust relations:
        <pre>{JSON.stringify(trustRelations, null, 2)}</pre>
    </div>
    <div>
        Recent transactions:
        <pre>{JSON.stringify(transactionHistory.currentPage.results, null, 2)}</pre>
    </div>
{/if}
