<template>
  <div class="content-wrapper">
    <h1>NFT Collection</h1>
    <table class="nft-grid">
      <tr v-for="(nftRow, rowIdx) in nftGridRows" :key="rowIdx">
        <td v-for="(nftItem, itemIdx) in nftRow" :key="itemIdx">
          <div v-if="nftItem" class="nft-card">
            <img :src="nftItem.uriData.image" alt="NFT Visual" @click="openImage(nftItem.uriData.image)" />
            <div class="nft-info">
              <div class="nft-details">
                <div class="metadata">
                  <h3>Metadata</h3>
                  <a :href="nftItem.metadata.uri" target="_blank">View Metadata</a>
                </div>
                <div class="transaction">
                  <h3>Transaction Details</h3>
                  <a :href="`https://amoy.polygonscan.com/tx/${nftItem.txhash}`" target="_blank">View Transaction</a>
                </div>
              </div>
            </div>
          </div>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      api_endpoint: 'https://api.vottun.tech/erc/v1/erc721/tokenUri',
      api_key: 'eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiIyaXpJQ0JhaTFpR1R2Nko5S2NzQmhyU05wNEsiLCJ0eXBlIjoiZXJwIiwiaWQiOiIiLCJ1c2VybmFtZSI6ImVyaWNrZGFuaWVsdG95b21hcmluMTBAZ21haWwuY29tIiwiY2lkIjoiN2JkNGI2NDgtNDdiMy00OGRkLThmZGUtNDAyZTlmZWFjMjgyIiwic2t1IjpbeyJyIjoxMSwicyI6OCwiZSI6MH0seyJyIjoxMSwicyI6ODAwMSwiZSI6MH0seyJyIjoxMSwicyI6ODAwMywiZSI6MH0seyJyIjoxMSwicyI6ODAwNSwiZSI6MH0seyJyIjoxMSwicyI6ODAxMCwiZSI6MH0seyJyIjoxMSwicyI6ODAwMiwiZSI6MH0seyJyIjoxMSwicyI6MywiZSI6MH1dLCJwdWMiOiIwMDAwMDAwMC0wMDAwLTAwMDAtMDAwMC0wMDAwMDAwMDAwMDAifQ.KM1ExMDc03J52r-CYNj_dhfqACO0XcbYqhVo3118CyIctAVxg9e33VEwFFHkYxj93a3tBruOwHLEgbd1Iqimei4BprtYrCiq02iFOW-DhIyrpqYHIxVLN-Y-hwFQAH465kGkoYZanOae6325DrUN3GDsBBsYZSblvs8_-mDNA_cpYsIS33h-ORh6dllUKubHRpsKlq83gfi7CgMHPWpPobrr626z7za9xGULlK5wYXU7xsKjffBZWRaoXrqSC-gCZdK9FjA0LFEy-v-zt4TmPf38XB4y5vPtdn6vUuD-Yqgh0aP7MlkJpQaGQFPzH9SXnktKhAs3fhJz49vtdGLACJb0iGkSzfUbHrDsiXX-_jjBx4AGe_-NvpCtJtY3RL7gS0dRZiciuGsi-eYto36YkndIhOIcjyZAt7Zdsj5cTeyDVudoRoPBHWCrRFeUr2Ip8badrei2CmSen2Bebm9uMYGmY--1IkaBboEiHur9RLIA0gTbpWita34X5cYi_7W4SMpJqZYrsvXJT4Q9QoiqHs6_mprCymR6sosOaYIeaDjI_RvVc9gXJnoY1K-i-0FYlD0wScgLJpZkXZewV2zO_B_JYi7vb8QW8wgHq0U98Wlwg7_uEDEY8F9hEuAOBPaP_kIVp60tqftyx-Po3M5vNGARmelUqKPd4ILXvdRzOPI',
      app_id: '7CPLqio6lhsGzrnoyoPTB0jp0Bvpte_YAz7xloGJ004vEwgbkIbKXj2Y94vjUhTV',
      contract_address: "0xDCffa9caF3b53eb549aB10A225Ef108E2E0717B3",
      network: 80002,
      ids_hashes: {
        10: "0x4fb010892c71c970ec3a975d77b29ec0b69505da7c948c6a3d01dcfe962d3660",
        11: "0xd7b468efa327533524b4cb12d322fcdb125b406020e439aa0b2b1b3c05b954ff",
        12: "0x9f1ce9965aca424783134c326465af03ebc504b38b85305f8baac34ccb528c84",
        13: "0xef85973c1ae6ce69e29aabe7cc3e9f18e8a95c4e2525ccd8d1d8d40d20ecc163",
        14: "0xaa835f0d97a615e13773a01220fc16b3c86ca1969686b67babeb54de0e93f09f",
        15: "0x5462626fc2aa6fe02fba8300a7b050c0407e283deb20a20341bd0c7cea62c02c",
        16: "0x3b0b2edba480ab0c99cf2b335be932dafd425d6b7277dd373bc40a5ee3e09b4d",
        17: "0xf7dd54bf256ad701b85bdb340fef399cbaba1475fb8d57ab9d41148435cf85f2",
        18: "0xb1b6e40d74f83d672124cfb4180b6fc6cf72413a7db60694bea892eeeaca5db9",
        19: "0x769dbb4e935b14d1d3f2d427c3673e6280bc62963f6c6ca1116aead230fd39b9"
      },
      nft_items: []
    };
  },
  computed: {
    nftGridRows() {
      const gridRows = [];
      for (let i = 0; i < this.nft_items.length; i += 3) {
        gridRows.push(this.nft_items.slice(i, i + 3));
      }
      return gridRows;
    }
  },
  methods: {
    async fetchMetadata(requestPayload) {
      try {
        const response = await fetch(this.api_endpoint, {
          method: "POST",
          headers: {
            'Authorization': `Bearer ${this.api_key}`,
            'x-application-vkn': this.app_id,
            "Content-Type": "application/json"
          },
          body: JSON.stringify(requestPayload)
        });

        if (!response.ok) {
          throw new Error('Failed to fetch metadata');
        }

        return await response.json();
      } catch (error) {
        console.error('Metadata fetch error:', error);
        throw error;
      }
    },
    async fetchUriData(uri) {
      try {
        const response = await fetch(uri);

        if (!response.ok) {
          throw new Error(`Failed to fetch URI data: ${uri}`);
        }

        return await response.json();
      } catch (error) {
        console.error('URI fetch error:', error);
        throw error;
      }
    },
    async loadNftItems() {
      try {
        const requestPayloads = Object.keys(this.ids_hashes).map(id => ({
          contractAddress: this.contract_address,
          network: this.network,
          id: parseInt(id)
        }));
        
        for (let i = 0; i < requestPayloads.length; i++) {
          const metadata = await this.fetchMetadata(requestPayloads[i]);
          const uriData = await this.fetchUriData(metadata.uri);
          this.nft_items.push({ uriData, metadata, txhash: this.ids_hashes[requestPayloads[i].id] });
        }
      } catch (error) {
        console.error('Error loading NFTs:', error);
      }
    },
    openImage(imgUrl) {
      window.open(imgUrl, '_blank');
    }
  },
  mounted() {
    this.loadNftItems();
  }
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  background-image: url('https://ipfsgw.vottun.tech/ipfs/bafkreifztrs2eolmvj6rmrfygncu4pkvek4ewovvgrnwlavhjilw57m6oa');
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center;
  background-attachment: fixed;
  color: #333;
  margin: 0;
  padding: 0;
}

.content-wrapper {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  background-color: rgba(255, 255, 255, 0.6);
  border-radius: 10px;
}

h1 {
  text-align: center;
  margin-bottom: 20px;
  font-size: 3.5em;
}

.nft-grid {
  width: 100%;
  border-collapse: collapse;
}

.nft-grid td {
  padding: 15px;
  text-align: center;
  vertical-align: top;
}

.nft-card {
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease-in-out;
}

.nft-card:hover {
  transform: translateY(-5px);
}

.nft-card img {
  width: 100%;
  height: auto;
  display: block;
  border-bottom: 1px solid #ddd;
  cursor: pointer;
}

.nft-info {
  padding: 15px;
}

.nft-details {
  display: flex;
  justify-content: space-between;
}

.metadata, .transaction {
  width: 48%;
}

.metadata h3, .transaction h3 {
  margin-bottom: 10px;
}

.nft-link a {
  color: #007bff;
  text-decoration: none;
}

.nft-link a:hover {
  text-decoration: underline;
}
</style>

