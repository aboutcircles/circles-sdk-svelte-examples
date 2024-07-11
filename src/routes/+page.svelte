<script lang="ts">
    import {Sdk, type ChainConfig, type SdkContractRunner} from "@circles-sdk/sdk";
    import {BrowserProvider} from "ethers";
    import {onMount} from "svelte";

    let avatarAddress = "0xb235B56b91eccb9DbdF811D7b5C45c363AcaE98D";
    let avatarType = "";

    const chainConfig: ChainConfig = {
        pathfinderUrl: 'https://pathfinder.aboutcircles.com',
        circlesRpcUrl: "https://rpc.helsinki.aboutcircles.com",
        v1HubAddress: "0x29b9a7fbb8995b2423a71cc17cf9810798f6c543"
    };

    async function getRunner() : Promise<SdkContractRunner> {
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
        const sdk = await getSdk();
        const avatar = await sdk.getAvatar(avatarAddress);

        avatarType = avatar.avatarInfo?.type;
    });
</script>
<p>
    Avatar {avatarAddress} is {avatarType}
</p>