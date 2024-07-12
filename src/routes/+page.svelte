<script lang="ts">
    import {BrowserProvider} from 'ethers';
    import {onMount} from 'svelte';

    let provider: BrowserProvider | undefined;
    let w: any;

    onMount(() => {
        w = <any>window;
        if (typeof w.ethereum !== 'undefined') {
            provider = new BrowserProvider(w.ethereum);
        } else {
            console.error('MetaMask is not installed.');
        }
    });

    async function addGnosisChain() {
        await addNetwork({
            chainId: '0x64', // 100 in hexadecimal
            chainName: 'Gnosis Chain',
            nativeCurrency: {
                name: 'xDAI',
                symbol: 'xDAI',
                decimals: 18
            },
            rpcUrls: ['https://rpc.gnosischain.com'],
            blockExplorerUrls: ['https://gnosisscan.io/']
        });
    }

    async function addNetwork(networkData: any) {
        if (!provider) {
            throw new Error('MetaMask is not installed.');
        }

        await w.ethereum.request({
            method: 'wallet_addEthereumChain',
            params: [networkData]
        });

        console.log('Network added successfully!');
    }
</script>
<h1>Circles SDK Examples</h1>
<ul>
    <li>
        <a href="using-avatars">Using avatars</a><br/>
        <p>
            This example signs a MetaMask EOA up as a human avatar and then loads the avatar info.
        </p>
        <p>
            <i>See <a target="_blank"
                      href="https://docs.aboutcircles.com/developer-docs/getting-started-with-the-sdk/creating-an-avatar">Using
                avatars</a> in the SDK documentation for more information.</i>
        </p>
    </li>
    <li>
        <a href="avatar-information">Avatar information</a>
        <p>
            This example loads the avatar info of the MetaMask EOA. The information includes the avatar type, total
            balance, mintable amount, trust relations, and transaction history.
        </p>
        <p>
            <i>See <a target="_blank"
                      href="https://docs.aboutcircles.com/developer-docs/getting-started-with-the-sdk/creating-an-avatar/avatar-information">Avatar
                information</a> in the SDK documentation for more information.</i>
        </p>
    </li>
    <li>
        <a href="interact-with-other-avatars">Interact with other avatars</a>
        <p>
            This example allows you to trust another avatar, mint personal tokens and transfer them to another avatar.
        </p>
        <p>
            <i>See <a target="_blank"
                      href="https://docs.aboutcircles.com/developer-docs/getting-started-with-the-sdk/creating-an-avatar/interact-with-other-avatars">Interact
                with other avatars</a> in the SDK documentation for more information.</i>
        </p>
    </li>
</ul>
<p>
    <button on:click={addGnosisChain}>
        Add Gnosis Chain to MetaMask
    </button>
</p>