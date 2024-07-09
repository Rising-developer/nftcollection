# nftcollection

This is a Vue.js (Vue3) project to display an NFT collection with the help of Vottun APIs.

## Prerequisites

- Node.js (>= 12.x)
- npm (>= 6.x) or yarn (>= 1.x)
- Vue CLI (>= 4.x)
- git

## Project Setup

1. Clone the Repository:
   git clone https://github.com/Rising-developer/nftcollection
   cd nftcollection

2. Install Vue CLI (if not already installed):
   npm install -g @vue/cli
   or
   yarn global add @vue/cli

3. Install Dependencies:      
   npm install
   or
   yarn install

4. Run the Development Server:
   npm run serve
   or
   yarn serve

5. Open the App:
   Open your browser and navigate to http://localhost:8080 (or the URL provided in the console).

## Building for Production (optional)

To build the project for production, run:

   npm run build
   or
   yarn build

## Linting and fixing files (optional)
   npm run lint

if you want to learn more about Vue CLI, go to: https://cli.vuejs.org/config/

## Configuration

Update the following variables in the "src/components/my_nft_collection.vue" file with your data:

    api_key: <'your-api-key'>,
    app_id: <'your-app-id'>,
    contract_address: <'the-contract-address'>,
    network: <the-network-id>,
    //id and txhash for each nft
    ids_hashes: {<id>: <'txhash'>}

Depending on the blockchain network you are going to use, you will have to update the link in this tag in the "my_nft_collection.vue" file:

 <a :href="`https://amoy.polygonscan.com/tx/${nftItem.txhash}`" target="_blank">View Transaction</a>

Additionally, you can use your own background image (with a Vottun ipfs link for example). For that you need to update the following line in the style section of the "my_nft_collection.vue" file:

   background-image: url('https://ipfsgw.vottun.tech/ipfs/bafkreifztrs2eolmvj6rmrfygncu4pkvek4ewovvgrnwlavhjilw57m6oa');
