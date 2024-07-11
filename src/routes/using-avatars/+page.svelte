<!-- Svelte demo code for the "Using avatars" section of the Circles SDK documentation -->
<script lang="ts">
    import {Sdk, type ChainConfig, type SdkContractRunner, type AvatarInterface} from "@circles-sdk/sdk";
    import {BrowserProvider} from "ethers";
    import {onMount} from "svelte";

    let avatar: AvatarInterface | undefined;
    let sdk: Sdk | undefined;
    let error: Error | undefined;

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

    async function registerAvatar() {
        error = undefined;
        try {
            avatar = await sdk?.registerHuman();

            // If you want to sign up an organization:
            // avatar = await sdk?.registerOrganization();
        } catch (e) {
            error = <any>e;
        }
    }

    async function loadAvatar() {
        error = undefined;
        try {
            avatar = await sdk?.getAvatar(sdk?.contractRunner.address);
        } catch (e) {
            error = <any>e;
        }
    }
</script>
{#if error}
    <p style="color: darkred">
        Error: {error.message}
    </p>
{/if}
{#if !avatar}
    <button on:click={registerAvatar}>
        Register Human
    </button>
    <button on:click={loadAvatar}>
        Load avatar
    </button>
{:else}
    <p>
        Avatar {avatar.avatarInfo?.avatar} is {avatar.avatarInfo?.type}
    </p>
{/if}