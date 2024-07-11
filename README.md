# Circles SDK Svelte Examples

This is a SvelteKit project that demonstrates how to use the Circles SDK. It is referenced by the [Circles SDK documentation](https://docs.aboutcircles.com/developer-docs/circles-sdk-overview).

The examples are bare bones, self-contained and are targeted to be run in a browser with MetaMask installed. The examples are:
* **[Using avatars](./src/routes/using-avatars/+page.svelte)**  
  This example demonstrates how to use the Circles SDK to sign up at Circles and display basic information about the avatar. _(see [Using Avatars](https://docs.aboutcircles.com/developer-docs/getting-started-with-the-sdk/creating-an-avatar) in the docs)_
* **[Avatar information](./src/routes/avatar-information/+page.svelte)**  
  This example demonstrates how to use the Circles SDK to get information about an avatar. The information includes:
  * Balance
  * Mintable amount
  * Trust relations
  * Transaction history    
  _(see [Avatar information](https://docs.aboutcircles.com/developer-docs/getting-started-with-the-sdk/creating-an-avatar/avatar-information) in the docs)_
* **[Interact with other avatars](./src/routes/interact-with-other-avatars/+page.svelte)**  
  This example demonstrates how to use the Circles SDK to interact with other avatars. The interactions include:
  * Trusting and untrusting another avatar
  * Minting personal Circles
  * Sending tokens to another avatar  
  _(see [Interact with other avatars](https://docs.aboutcircles.com/developer-docs/getting-started-with-the-sdk/creating-an-avatar/interact-with-other-avatars) in the docs)_

## Run

### 1. Pre-requisites
* Install [Git](https://git-scm.com/downloads)
* Install [Node.js](https://nodejs.org/en/download/)
* Install [MetaMask](https://metamask.io/download/) and create an account
* Get some [xDAI](https://docs.gnosischain.com/about/tokens/xdai) (e.g. from a [faucet](https://docs.gnosischain.com/tools/Faucets)) and send it to your MetaMask account
### 2. Clone the repository
```bash
git clone .. 
```

### 3. Install dependencies
```bash
cd circles-sdk-svelte-examples
npm install
```

### 4. Start the development server
```bash
npm run dev -- --open
```